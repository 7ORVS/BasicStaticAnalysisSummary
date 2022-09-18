# Basic static analysis summary

In this phase of malware analysis, we take a first look at our sample,  we can get beneficial information that will help us in other phases

## 1- Hashes & Virustotal

In this step, we will get the sample sha256 for example hash with any tool to do that, for me i use **sha256sum.exe** builtin tool in **cmder** 

![](/pics/Screenshot%20from%202022-09-18%2013-42-03.png)

So, now we got the file hash. <br>

Next step we will take this value and search about it in virustotal in this website you can search about anything you you suspect may be harmful like URLs, files and hashes. <br>

So, we get this information
![](/pics/vt1.png) <br>
Some useful information about this sample <br>
And this important section, some of imports that may be harmful
![](/pics/warm_imports.png)
<br>

The second step, we will extract the strings from malware binaries, this strings will be very useful for next analysis phases, we will use tool called **FLOSS**, you can know more about this tool on this link
https://github.com/mandiant/flare-floss 
<br>

![](/pics/floss.png)
<br>
![](/pics/floss_output.png)
<br>
These strings look unreadable but if we scroll down a little bit we will find some useful ones
![](/pics/floss_imp_output.png)
We find this cmd command, url, path etc.<br>
So we can take these strings as notes to see it in onther phase. <br>

So, now we have sample hash and some information from virustotal and also we extraced strings from this binary. <br>

Now, we will see something a bit deep.<br>
We want to know how this sample operate with out os but we will not run, we define this behavior with the APIs called by this sample and also our sample structure.<br>

For this we will use a  tool provided by called **PEview** you can know more about it in this link <br>
http://wjradburn.com/software/

![](/pics/PEview.png)
We will see some raw data in hex and its values 

We can jump to our important section. <br>    
    
    1- Time Date Stamp:
       this is time of compilation of this sample
       ![](/pics/time_stamp.png)
       Note: Delphi compiler will always have a timestamp of 1992 
    
    2- Virtual size & Size of raw data: 
       in IMAGE_SECTION_HEADER.text which has information that can ber read into the binary at runtime, in this section we will care about the virtual size which represents the actual size of data on the disk when the binary is running and size of raw data and compare between these values if these values closing to each other so we can conclude that this malware is unpacked.<br>
    3- The last part we will look at is tool, this section contain all Windows APIs called by binary 
    ![](/pics/import_table.png)
    and with these APIs we can know some of function that this sample may be do like this API:
    ![](/pics/url_to_file.png)
    if we search about this API in microsoft document we will find that this API download a file from certain url and write it to disk.

    Note: we can from this tool to determine if this sample is packed or unpacked by looking at import address table which will contain few amount of APIs and the virtual size and the size of raw data will not close to each other

This information will be useful to draw our road on next phases of analysis. <br>

I would like to end this summary with important tool that combines all of previous steps into on tool called **PEstudio** you can learn more about it in this link
https://www.winitor.com/

This tool can get the file hash, file type and arch, frequently API used in harmful things, strings and security indicators sorted by its level. <br>

![](/pics/pes_hashes.png)<br>
![](/pics/pes_indicators.png)<br>
![](/pics/pes_apis.png)<br>
![](/pics/pes_strings.png)<br>

All of this information we can get only from **PEstudio**, so this is a highly recommended tool to use in this phase.<br>

And here we finished this phase to go through another phase.<br>


