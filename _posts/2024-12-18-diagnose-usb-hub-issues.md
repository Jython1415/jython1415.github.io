---
title: "Diagnosing slow data transfer rates for my USB C hub"
categories:
  - Blog
tags:
  - Tech
  - Programming
---

Data transfer to my 1TB SSD felt abnormally slow with my USB C hub, but really fast with my girlfriend's hub. On top of that, when I was using my USB C hub, my monitor would occasionally cut out, but with my girlfriend's, I never had that issue.

I *suspect* the hub is the issue, but I'm going to try to test that by measuring data transfer rates. I really wouldn't want the physical transfer rate to be limiting anything I'm working on.

# Writing a benchmarking script

I asked ChatGPT for help with this, and they suggested I use the `dd` command to write a test file on my 1TB SSD. The `dd` command measures how long that takes, which is pretty neat.

```bash
$ dd if=/dev/zero of=/Volumes/1TB\ Manual/testfile bs=1m count=4096
4096+0 records in
4096+0 records out
4294967296 bytes transferred in 129.563073 secs (33149625 bytes/sec)
```

The value we're really interested in is the data transfer rate, `33149625` in this case, so we can pipe the results into `awk`.

```bash
$ dd if=/dev/zero of=/Volumes/1TB\ Manual/testfile bs=1m count=4096 2>&1 | awk '/bytes\/sec/ {gsub(/[^0-9]/, "", $(NF-1)); print $(NF-1)}'
33025085
```

Finally, we can divide that by $1024^2$ to get the result in $\text{MB}/\text{s}$.

```bash
$ dd if=/dev/zero of=/Volumes/1TB\ Manual/testfile bs=1m count=4096 2>&1 | awk '/bytes\/sec/ {gsub(/[^0-9]/, "", $(NF-1)); print $(NF-1)}' | bc -l <<< "scale=2; $(cat) / 1048576"
```

This is getting a little complicated, and I want to run the same test for reading too, so I'll stick this logic in a script with ChatGPT's help. The script adds the following:

1. Read test
2. Cleaning up the test file
3. Passing in the path as an argument

```zsh
#!/usr/bin/env zsh --no-rcs

# Check if volume path argument is provided
if [[ -z "$1" ]]; then
    echo "Usage: $0 <path-to-volume>"
    exit 1
fi

# Validate the provided path
if [[ ! -d "$1" ]]; then
    echo "Error: '$1' is not a valid directory or does not exist."
    exit 1
fi

# Define test file path and size
VOLUME_PATH="$1"
TEST_FILE="$(mktemp "$VOLUME_PATH/testfile.XXXXXX.tmp")"
TEST_SIZE="4096"  # Size in MiB (4GB)

# Function to clean up the test file
function cleanup() {
    if [[ -f "$TEST_FILE" ]]; then
        echo "Cleaning up test file..."
        rm -f "$TEST_FILE"
    fi
}

# Set up traps to ensure cleanup
trap cleanup EXIT         # Run cleanup on script exit
trap cleanup INT TERM HUP # Run cleanup on interruption (Ctrl+C, termination, or hangup)

# Function to run dd and extract speed in MB/s
function run_dd_test() {
    local direction=$1  # "write" or "read"
    local dd_command=$2
    echo "Running $direction test..."

    # Run dd, extract speed in bytes/sec, and convert to MB/s
    SPEED=$(eval "$dd_command" 2>&1 | \
        awk '/bytes\/sec/ {gsub(/[^0-9]/, "", $(NF-1)); printf "%.2f", $(NF-1) / 1048576}')
    
    echo "$direction speed: $SPEED MB/s"
}

# Perform write test
run_dd_test "write" "dd if=/dev/zero of='$TEST_FILE' bs=1m count=$TEST_SIZE"

# Perform read test
run_dd_test "read" "dd if='$TEST_FILE' of=/dev/null bs=1m"

echo "Benchmark completed."
```

Now, I just have to put this in my ["dotfiles" repository](https://github.com/Jython1415/dotfiles) and use it to figure out what's going on with the hub.

# Recording results from different connections

I'm currently using an [M2 MacBook Air](https://en.wikipedia.org/wiki/MacBook_Air_(Apple_silicon)#M2_and_M3_(2022–present)). There are 2 USB C ports on the left side, so I'll refer to them as the "A" and "B" ports, with A being closer to the charging port. My USB C hub (An older version of the [Chosure USB hub](https://a.co/d/eULor79) ([archive.today](http://archive.today/TAT5z))) has 3 USB A ports, and I'll refer to them as "1", "2", "3", with "1" being closest to the USB C port, and "3" being furthest. Here are the read–write speeds I recorded for each of them.

| Connection | Write | Read  |
| ---------- | ----- | ----- |
| A1         | 32.29 | 31.90 |
| A2         | 32.41 | 31.70 |
| A3         | 32.62 | 31.70 |
| B1         | 31.54 | 31.44 |
| B2         | 31.82 | 31.52 |
| B3         | 31.48 | 31.47 |

Read speeds seem *marginally* faster on A than B. Other than that, I don't see any differences that I'd suspect would turn out to be statistically significant with further testing.

Since one of my initial issues was with when I had my display connected as well, I'm going to repeat the test on A1 with my display connected and playing something, to see if that changes the transfer rate at all.

| Connection | Write | Read  |
| ---------- | ----- | ----- |
| A1         | 32.59 | 31.88 |
| A2         | 32.44 | 31.82 |
| A3         | 32.41 | 31.83 |

No real difference, and I'm still getting the display issue like before. I'll have to try with my girlfriend's USB C hub and see if that makes any difference. She has a "ANKER PowerExpand Direct 7-in-2 USB-C PD Media Hub". The hub plugs into both USB ports on my MacBook Air, so no A/B distinction this time. There are two USB A ports, so I'll be testing both of them, starting with the one closer to the HDMI port.

This test is with the display connected as well.

| Connection | Write  | Read     |
| ---------- | ------ | -------- |
| 1          | 385.43 | 374.41   |
| 2          | 394.25 | 14614.17 |

The read speed seems outlandish? Like is there some sort of caching going on that is messing with the test? I'm going to record the values anyway, but I'm suspicious of their validity.

# Comparing results with the advertised values

With the ANKER hub plugged in, I see "Up to 5 Gb/s" for System Information > Hardware > USB > USB 3.1 Bus > USB3.0 Hub > Speed.

With The Chosure hub plugged in, I see "Up to 480 Mb/s" for System Information > Hardware > USB > USB 3.1 Bus > USB 2.1 Hub > Speed.

So, there's a 10x difference in speed, for one, and the Chosure USB is listed as a USB 2.1 hub instead of a 3.1 hub. I'm not sure what the significance of that is.

The Chosure hub seems 10x slower than their advertised 5 Gb/s, and the ANKER hub seems to be inline with their promotional material. But, as I did recognize earlier, the version of the Chosure hub I got is older than what they currently have on Amazon, so there is a chance that the newer version is just faster, and mine is performing as well as it was intended to.

# Conclusion

- Having a tool to benchmark issues like this will be handy in the future.
- I should probably get another hub... after checking their advertised data transfer rate more carefully.
- Something is definitely going with the read test on the 2nd port of ANKER. I should follow-up on that... If I get around to it, I'll make another post out of it.
