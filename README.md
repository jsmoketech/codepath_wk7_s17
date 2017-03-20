# Project 7 - WordPress Pentesting

Time spent: **11** hours spent in total

> Objective: Find, analyze, recreate, and document **five vulnerabilities** affecting an old version of WordPress

## Pentesting Report

1. (Required) Title: WordPress 2.5-4.6 - Authenticated Stored Cross-Site Scripting via Image Filename
  - [x] Summary: By uploading an image with a malicious name, which contains a script, it will execute upon loading the attachment page where the image is stored
    - Vulnerability types: XSS
    - Tested in version: 4.2.2
    - Fixed in version: 4.7 
  - [x] GIF Walkthrough:  <img src='https://github.com/santis25/codepath_wk7_s17/blob/master/codepath_websec_w07_lab.gif' title='Video Walkthrough' width='' alt='Video Walkthrough' />
  - [x] Steps to recreate: 
  	A. Admin will upload an image, presumably given to him/her by a trusted user or popular image via social websites.
  	B. This image will contain a malicious filename.
  	C. When the attachment page is visited the filename is not santized resulting in the page executing the script contained in it.
  	D. URL of attachment page is now usable as a way to run script on other computers.
  - [x] Affected source code: Santization isn't done in media.php file. This is later patched to santize the filename.
    - [media.php](https://core.trac.wordpress.org/browser/branches/4.2/src/wp-admin/includes/media.php)
2. (Required) Title: WordPress <= 4.3 - Authenticated Shortcode Tags Cross-Site Scripting (XSS)
  - [x] Summary: Takes advantage of how shortcode and links are validated, to insert malicious javascript
    - Vulnerability types: XSS
    - Tested in version: 4.1
    - Fixed in version:  4.3
  - [x] GIF Walkthrough: <img src='https://github.com/santis25/codepath_wk7_s17/blob/master/codepath_websec_w07_attack02.gif' title='Video Walkthrough' width='' alt='Video Walkthrough' />
  - [x] Steps to recreate: 
  	A. Create a Post and enter the following into the field: 
```javascript
[caption width='1' caption='<a href="' ">]</a><a href="onmouseover='alert(1)'">
```
  - [x] Affected source code: Post.php does not filter both shortcode and html well in tandem
    - [post.php](https://core.trac.wordpress.org/browser/branches/4.1/src/wp-includes/post.php)
3. (Required) Title: WordPress  4.0-4.7.2 - Authenticated Stored Cross-Site Scripting (XSS) in YouTube URL Embeds
  - [x] Summary: Take advantage of youtube embedding to execute a XSS attack 
    - Vulnerability types: XSS
    - Tested in version: 4.1
    - Fixed in version: 4.7.3
  - [x] GIF Walkthrough: <img src='https://github.com/santis25/codepath_wk7_s17/blob/master/codepath_websec_w07_attack03.gif' title='Video Walkthrough' width='' alt='Video Walkthrough' />
  - [x] Steps to recreate: 
  	A. Create a new post, requires Contributor privelege or higher
  	B. Add this to the post: 

    [embed src='https://youtube.com/embed/12345\x3csvg onload=alert(1)\x3e'][/embed]

    C. When a user visits the page xss attack occurs.
  - [x] Affected source code:
    - [media.php](https://core.trac.wordpress.org/browser/branches/4.1/src/wp-includes/media.php)
4. (Optional) Title: WordPress 4.1 Broke down deletion functionality via Theme Name fallback
  - [x] Summary: Made a theme undeletable via wordpress gui
    - Vulnerability types: XSS, Linux
    - Tested in version: 4.1
    - Fixed in version: 4.8
  - [x] GIF Walkthrough: <img src='https://github.com/santis25/codepath_wk7_s17/blob/master/codepath_websec_w07_attack04.gif' title='Video Walkthrough' width='' alt='Video Walkthrough' />
  - [x] Steps to recreate: 
  	A. Downloaded a theme of compatible size for my version of wordpress (looked through theme shop to find candidates)
  	B. Instead of installing from theme shop downloaded via url, guessed at the source using the theme name and the version found in the shop in step A, to obtain source https://downloads.wordpress.org/theme/twentyfifteen.1.1.zip
  	C. Executed command to extract files: $ unzip twentyfifteen.1.1.zip
  	D. Modified second line in twentyfifteen/style.css  to &lt;a onload='alert(1)'&gt;
  	E. Renamed directory with:  mv twentyfifteen "&lt;a onload='alert(1)'&gt;"
  	F. Compressed directory into theme.zip:  zip -r theme.zip "&lt;a onload='alert(1)'&gt;"
  	G. Then simulated attack by logging in as Administrator
  	H. Navigated to the theme page and uploaded theme.zip which another user(attacker) sent me
  	I. After uploading Noticed this theme has been tampered with, and attempted to delete.
  	J. Theme can not be deleted by wordpress admin suite.
  - [x] Affected source code:
    - [theme.php](https://core.trac.wordpress.org/browser/tags/4.1/src/wp-includes/theme.php?rev=30975)
5. (Optional) Vulnerability Name or ID
  - [ ] Summary: 
    - Vulnerability types:
    - Tested in version:
    - Fixed in version: 
  - [ ] GIF Walkthrough: 
  - [ ] Steps to recreate: 
  - [ ] Affected source code:
    - [Link 1](https://core.trac.wordpress.org/browser/tags/version/src/source_file.php) 

## Assets

List any additional assets, such as scripts or files

## Resources

- [WordPress Source Browser](https://core.trac.wordpress.org/browser/)
- [WordPress Developer Reference](https://developer.wordpress.org/reference/)

GIFs created with [SimpleScreenRecorder](http://www.maartenbaert.be/simplescreenrecorder/).

## Notes

Several of the documented attacks proved to be difficult for me to pull off. Although at first I spent much time trying to execute some of the non-XSS attacks, I could not pull off a successful attack, I believe this was mainly due to my unfamiliarity with wordpress, looking through the code I spent much time trying to see how I would attack these mistakes however it proved to take too much time seeing as how we have a time limit, I moved on to other attacks. This made executing the XSS attacks very simple however.

## License

    Copyright [2017] [Santiago Salas]

    Licensed under the Apache License, Version 2.0 (the "License");
    you may not use this file except in compliance with the License.
    You may obtain a copy of the License at

        http://www.apache.org/licenses/LICENSE-2.0

    Unless required by applicable law or agreed to in writing, software
    distributed under the License is distributed on an "AS IS" BASIS,
    WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
    See the License for the specific language governing permissions and
    limitations under the License.
