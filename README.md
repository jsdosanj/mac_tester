# mac_tester
Bash script that tests mac ssd read/write and batter

#!/bin/bash

# SSD Read/Write Test
function ssd_test() {
  echo "Running SSD Read/Write Test..."
  
  # Generate a temporary file of size 1GB
  dd if=/dev/zero of=/tmp/testfile bs=1M count=1000
  
  # Measure write speed
  echo "Measuring write speed..."
  write_speed=$(dd if=/tmp/testfile of=/dev/null bs=1M count=1000 2>&1 | awk '/copied/ {print $8 " " $9}')
  echo "Write speed: $write_speed"
  
  # Measure read speed
  echo "Measuring read speed..."
  read_speed=$(dd if=/dev/null of=/tmp/testfile bs=1M count=1000 2>&1 | awk '/copied/ {print $8 " " $9}')
  echo "Read speed: $read_speed"
  
  # Clean up temporary file
  rm /tmp/testfile
}

# Battery Test
function battery_test() {
  echo "Running Battery Test..."
  
  # Check battery percentage
  battery_percentage=$(pmset -g batt | awk '/\s[0-9]+%/ {print $2}')
  echo "Battery Percentage: $battery_percentage"
  
  # Check battery status
  battery_status=$(pmset -g batt | awk '/;/{gsub(/;/,""); print $4}')
  echo "Battery Status: $battery_status"
}

# Camera Test
function camera_test() {
  echo "Running Camera Test..."
  
  # Capture image using built-in camera
  image_file="/tmp/test_image.jpg"
  echo "Capturing image..."
  sudo killall VDCAssistant
  sleep 2
  sudo killall VDCAssistant
  sleep 2
  sudo killall VDCAssistant
  sleep 2
  sudo screencapture -x $image_file
  
  # Check if the image was captured successfully
  if [ -f "$image_file" ]; then
    echo "Image captured successfully: $image_file"
    open $image_file
  else
    echo "Failed to capture image."
  fi
}

# Run tests
ssd_test
battery_test
camera_test
