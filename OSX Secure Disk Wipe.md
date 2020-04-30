## Securely erase an external disk using `dd` on OSX

1. Plug in your SD card, HDD, or other block device and then use the following command to see which /dev/diskN node it's located on:

  ```bash
  diskutil list
  ```

2. Unmount the disk where N is the number of the disk taken from the above command:

  ```bash
  diskutil unmountDisk /dev/rdiskN
  ```

  If the above command was successful, you will see:

  > Unmount of all volumes on diskN was successful

3. Execute `dd` command as super user on disk where "N" is the number of the disk from step 1.

  ```bash
  sudo dd if=/dev/urandom of=/dev/rdiskN bs=1000000
  ```
  
  or use diskutil cli
    ```bash
  sudo diskutil secureErase [0-4] /dev/rdiskN
  ```
  
    ```
        0 - Single-pass zeros.
        1 - Single-pass random numbers.
        2 - US DoD 7-pass secure erase.
        3 - Gutmann algorithm 35-pass secure erase.
        4 - US DoE 3-pass secure erase.
    ```
    
  This will overwrite all partitions, master boot records, and data.  Please note that this may take a while depending on the size of your disk and there is no progress indicator.  However; If you want to check whether or not `dd` is working you can always use `pv` ([Available on Homebrew](http://brew.sh)) which will dump out the raw data being written to the disk.
 
