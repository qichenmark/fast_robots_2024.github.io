---
layout: default
---
          

**Part 1**

·       **Example: Blink it Up**: verify and upload, and confirmed the light is blinking.

·       **Example4\_Serial**: change the baud to 9600, verify, and upload.

![A screenshot of a computer
Description automatically generated](lab1%20Write-up_files/image002.png)

·       **Example4\_Serial**: switch the serial monitor’s baud speed to 115200 baud, verify, and upload:

![A screenshot of a computer
Description automatically generated](lab1%20Write-up_files/image004.png)

after holding the board in hand for 60 seconds, temp increased significantly:

![A screenshot of a computer
Description automatically generated](lab1%20Write-up_files/image006.png)

·       **Example1\_MicrophoneOutput:**

          switch the serial monitor’s baud speed to 115200 baud, verify, and upload:

          ![A screenshot of a computer
Description automatically generated](lab1%20Write-up_files/image008.png)

          Scratch on the board, the sound’s frequency became:

          ![A screenshot of a computer
Description automatically generated](lab1%20Write-up_files/image010.png)

**Part 2**

**Goal**

          Understanding that Artemis can use Bluetooth to communicate with computer in terms of Characteristics; There’re multiple Characteristics, such as float Characteristics and string Characteristics. We can use ble.receive\_string(uuid) to receive a known message, but in lots of cases we don’t know if there is a message await us, so it’s better to use ble.start\_notify(uuid, notification\_handler), which continuously listen to messages from the given uuid.

**Prelab**

Following the instructions in Lab1 course page, running following commands in powershell to get the environment ready:

python -m pip install --user virtualenv

python -m venv FastRobots\_ble

For the older computer with both python 2 and 3, use ‘python3’ instead of ‘python’.

Once we have the environment, next is activate the Virtual environment by

.\\FastRobots\_ble\\Scripts\\activate

And install Juypter Notebook with

          pip install numpy pyyaml colorama nest\_asyncio bleak jupyterlab

Then simply put the given codebase into the environment, we are ready to start tasks.

·       **T1 Send ECHO command**

          ![A computer screen shot of a computer code
Description automatically generated](lab1%20Write-up_files/image011.png)

·       **T2. Programming and use GET\_TIME\_MILLIS command**

   ![A computer screen shot of a computer code
Description automatically generated](lab1%20Write-up_files/image012.png)

·       **T3. notification handler**

After setting up the notification handler, once Artemis send a message, Juypter notebook will immediately receive it and print in on screen; I used 1 if condition to determine incoming characteristic is string or float, another nested if condition for the time stamp and temperature extraction for later tasks.

![A screenshot of a computer code
Description automatically generated](lab1%20Write-up_files/image014.png)

For example, once CMD.GET\_TIME\_MILLIS lets the Artemis send the current time “T:215108.0”, Juypter notebook receive and print it.

·       **T4**. **Get current time stamps**

I implemented a function in ble\_arduino to send 100 time stamps. I didn’t choose larger amount of time stamps to avoid running of memory.

![A screen shot of a computer code
Description automatically generated](lab1%20Write-up_files/image016.png)

And when I sent the commend from python, 100 results are sent back from Artemis:

![A screenshot of a computer
Description automatically generated](lab1%20Write-up_files/image018.png)

Average data transfer rate = 47.755 strings/s = 334.3 bytes/s

·       **T5. Get, store, and sent time stamps--SEND\_TIME\_DATA command**

I implemented a function in ble\_arduino to send 105 time stamps. I used 15\*7 nested loop instead of 105 loop to overcome the Estring size limitation problem: in this case, the length of the Estring tx\_estring\_value is ensured to be less than 150 before I clear its content;

![A screenshot of a computer program
Description automatically generated](lab1%20Write-up_files/image020.png)

And when I sent the commend from python, 105 results are sent back from Artemis:

![A screenshot of a computer
Description automatically generated](lab1%20Write-up_files/image022.png)

Average data recording rate = 52500 strings/s = 420000 bytes/s

·       **T6. Send time& temperature--GET\_TEMP\_READINGS**

Added a nested if condition to extract time stamp and temperature from the string characteristic. I chose ‘;’ as delimiter because it’s not used in the raw data.

![A screenshot of a computer code
Description automatically generated](lab1%20Write-up_files/image024.png)

![A screenshot of a computer
Description automatically generated](lab1%20Write-up_files/image025.png)

Average data recording rate = 3225.8 strings/s = 48387.1 bytes/s

·       **T7. Compare and Contrast**

The first method(sending while reading data) is more than 1000 slower than the second method(recording all before sending data). If the memory of the board is very limited, the first method could be the preferred option; Otherwise, the second method is better since robots’ data are time-sensitive and a faster rate is desired in general.

386kB of RAM means the red board can store 48250 time stamps (assume time is 7 bytes long and delimiter is 1 byte long) or 55143 temperature readings (assume the reading is 6 bytes long and delimiter is 1 byte long).

**Conclusion, Challenges,** **and Solutions**

1.     When I try to implemented the notification handler, I didn’t realize that the uuid passed into the handler is GATT Characteristic rather than string, and that the way to get the string uuid is to uuid.uuid instead of uuid.uuid(), as .uuid is defined as a property, not a function; after reading forum and asking in Ed, I find these facts, and successfully implemented the notification handler which is able to deal both float and string.

2.     On task 5, my original codes was not able to print out anything except crush the programming and disconnect the board; after printing out values inside the loops, I realized that it’s caused by the 150 transmission length limitation; I resolved that by using a nested loop.

·       Through these tasks, I have successfully built up the Bluetooth connection between Artemis board and computer, enable the GATTCharacteristic data transmission, and found the limitation of Artemis’s transmission speed & storage.