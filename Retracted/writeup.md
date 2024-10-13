# RETRACTED

## TASK 1 - INTRODUCTION

A Mother's Plea

"Thanks for coming. I know you are busy with your new job, but I did not know who else to turn to."

"So I downloaded and ran an installer for an antivirus program I needed. After a while, I noticed I could no longer open any of my files. And then I saw that my wallpaper was different and contained a terrifying message telling me to pay if I wanted to get my files back. I panicked and got out of the room to call you. But when I came back, everything was back to normal."

"Except for one message telling me to check my Bitcoin wallet. But I don't even know what a Bitcoin is!"

"Can you help me check if my computer is now fine?"

## TASK 2 - The Message

"So, as soon as you finish logging in to the computer, you'll see a file on the desktop addressed to me."

"I have no idea why that message is there and what it means. Maybe you do?"

### Questions

> What is the full path of the text file containing the "message"?
>
I opened file explorer, went to desktop then clicked on the file, then I went to home in the menu bar and selected copy path

[assets/path.png]

ANS: C:\Users\Sophie\Desktop\SOPHIE.txt

> What program was used to create the text file?
>
Since it was a text file, I just guessed and I got it right

ANS: notepad.exe

>What is the time of execution of the process that created the text file? Timezone UTC (Format YYYY-MM-DD hh:mm:ss)
>
It was a process created by notepad so I decided to open up Event viewer, I filtered the log with Event ID 1 then I used the find button to search for notepad.exe and then I found it

[image]

ANS: 2024-01-08 14:25:30 

## TASK 3 - Something Wrong

"I swear something went wrong with my computer when I ran the installer. Suddenly, my files could not be opened, and the wallpaper changed, telling me to pay."

"Wait, are you telling me that the file I downloaded is a virus? But I downloaded it from Google!"

### Questions

> What is the filename of this "installer"? (Including the file extension)
>
Since the file was downloaded, I went to the downloads folder, it was empty. She said she downloaded from google so obviously she used a browser and then I went to open Microsoft edge and opened the downloads on the browser then I found two files but it was one she downloaded which is the antivirus

So yeah that is the filename of the installer

[image.png]

ANS: antivirus.exe

> What is the download location of this installer?
>
From the browwser, I opened the file location and then when I got there, I right clicked on the file and checked the properties, from there you will find the file path

[image.png]

ANS: C:\Users\Sophie\download

> The installer encrypts files and then adds a file extension to the end of the file name. What is this file extension?
>
I opened Event Viewer, I scrolled through the events and I saw that file created was event ID 11, I filtered the log with the event ID and then I decided to search for the installer using find

[image.png]

ANS: .dmp

> The installer reached out to an IP. What is this IP?
>
Event ID for network connection is 3 so i filtered the logs with 3 and then searched for the installler (antivirus.exe) and I clicked on the event and checked the destination ip address

[image.png]

ANS: 10.10.8.111

## TASK 4

"So what happened to the virus? It does seem to be gone since all my files are back."

### Questions

> The threat actor logged in via RDP right after the “installer” was downloaded. What is the source IP?
>
This is still a network connection so I left the filter on Event ID 3, then I searched for RDP using find, I clicked on details and found sourceIP, but I didn't get this on first try, I had to keep checking different RDP before I finally got the answer

[image.png]

ANS:10.11.27.46

> This other person downloaded a file and ran it. When was this file run? Timezone UTC (Format YYYY-MM-DD hh:mm:ss)
> 
I guess the other downloaded file we saw earlier is this file so I went to the event viewer and then searched for decryptor and since it was a process creation, I only checked for the event ID with process creation

[image.png]

ANS: 2024-01-08 14:24:18

## TASK 5 - Doesn't Make Sense

"So you're telling me that someone accessed my computer and changed my files but later undid the changes?"

"That doesn't make any sense. Why infect my machine and clean it afterwards?"

"Can you help me make sense of this?"

### Question: Arrange the following events in sequential order from 1 to 7, based on the timeline in which they occurred.

> ANSWERS
> 1. Sophie downloaded the malware and ran it.
> 2. The malware encrypted the files on the computer and showed a ransomware note.
> 3. Sophie ran out and reached out to you for help.
> 4. Someone else logged into Sophie’s machine via RDP and started looking around.
> 5. The intruder downloaded a decryptor and decrypted all the files.
> 6. A note was created on the desktop telling Sophie to check her Bitcoin.
> 7. We arrive on the scene to investigate.

## TASK 6 - CONCLUSION

"Adelle from Finance just called me. She says that someone just donated a huge amount of bitcoin to our charity's account!"

"Could this be our intruder? His malware accidentally infected our systems, found the mistake, and retracted all the changes?"

"Maybe he had a change of heart?"

Yeah, possibly.

And that is all, Thank you for reading!!



