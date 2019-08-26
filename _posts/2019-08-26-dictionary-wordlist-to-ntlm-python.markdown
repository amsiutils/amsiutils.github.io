---
layout: post
title:  "Python Dictionary Wordlist to NTLM Hash Rainbow Table"
date:   2019-08-26 00:00:00 -0400
categories: 
---

#### Large Wordlists

Create your own NTLM hash rainbow table with python. For large wordlists, split them into chunks with Text File Splitter, then join them back later. 

[Text File Splitter][tfs]

#### Wordlist to NTLM Hash

Change `your_file_path` and `your_result_path`.

Disable AV for a 10x performance boost.

```
import hashlib,binascii,sys,re
reload(sys)  
sys.setdefaultencoding('Cp1252')
your_file_path = "C:\\Temp\\Top2Billion-probable-v2_000010.txt" 
your_result_path = "C:\\Temp\\Top2Billion-probable-v2_000010h.txt" 

def md4_of_each_words(your_file_path):
    with open(your_file_path, "r") as raw_data:
        read_raw_data = raw_data.read()

    make_a_list = read_raw_data.split('\n')
    for lines in make_a_list:
		try:
			lines = unicode(lines, 'utf-8')
			last_md4 = hashlib.new('md4', lines.encode('utf-16le')).digest()
			with open(your_result_path, "a") as md4_list:
				md4_list.write("\"" + lines + "\",\"" + binascii.hexlify(last_md4) + "\"\n")
		except:
			#print words
			pass

    print "Completed."

md4_of_each_words(your_file_path)
```

[tfs]: http://textfilesplitter.org/download.html
