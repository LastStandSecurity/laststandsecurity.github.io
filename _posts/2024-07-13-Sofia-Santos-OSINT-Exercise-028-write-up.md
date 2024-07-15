---
title: Sofia Santos OSINT Challenge 028 write-up
categories: [writeups,OSINT]
comments: true
---

As well as Cyber Security Operations, one of my other passions is OSINT. I've also recently been learning about [skill stacking](https://hive.com/blog/skill-stacking/), which is a really intriuging concept and something I'm now actively trying to practice. With that in mind, I'm always looking for ways to expand and practice my OSINT skills. One of the tools I use for this is the excellent [OSINT challenges](https://gralhix.com/list-of-osint-exercises/) made by [Sofia Santos](https://x.com/Gralhix). 

Every so often she posts a new challenge with some preliminary information and a set of questions to solve. Recently she posted [OSINT Challenge 028](https://gralhix.com/list-of-osint-exercises/osint-exercise-028/). As I've not published anything for far too long, I decided to write a blog post explaining how I solved the challenge. 

## The challenge

In this challenge, we are given a picture of what looks to be an abandoned building surrounded by quite a lot of foliage. There's also a green gate with the number 55 on it and what looks to be either a green waste bin or a postbox. These small details, eg, the colour of the gate, the number on it, the foliage, are all important things to notice, as they might come in handy for the rest of the challenge.

![OSINT Challenge 028 photo](/assets/img/sofiasantos-028.jpg) 

We are also given the date of when the photo was taken - *20 September 2023, at 08:11 local time*. This information is helpful for future parts of the challenge and is probably given here to help beginners, but it's worth noting it can also be discovered when answering one of the challenge questions. 

The four questions posed for this challenge are:

* a) What device I used to take the photograph?
* b) Where I was headed?
* c) How far I was from the entrance of my destination?

* Bonus challenge: In which hotel did I stay?

For each challenge, Sofia always rates the perceived difficulty of the challenge. For this particular challenge, these are rated as:

* For beginners: a) Medium, b) Easy, c) Medium.
* For experts: a) Easy, b) Easy, c) Easy.

Whilst I don't consider myself an OSINT expert by any means, I did find most of these fairly easy except the bonus question. Sometimes the difficulty mainly depends on how recently you've used a particular set of skills - happily for me it's not long since I've had to use the skills listed below, so that probably explains why I didn't struggle too much here. The rest of this blog shows my methodology for answering each of these questions. 

## Question 1 - What device I used to take the photograph?
This question is pretty straightforward really for anyone who may have ever done any photo editing or has a large photo library, or of course anyone interested in OSINT. 

All photos taken by modern devices store [EXIF data](https://en.wikipedia.org/wiki/Exif) within the image. This can be very useful during OSINT investigations to establish when a photo was taken or the device used. In some cases, if the device used supports this and it is enabled, EXIF data can even store the geo co-ordinates for where the image was taken. 

If you're thinking all of this sounds a bit dystopian and could be misused, you'd be right. Fortunately, major social media providers (facebook, twitter, instagram) are all wise to this and remove EXIF data from images when posted <https://exifviewerapp.com/exif-data-in-social-media-what-you-need-to-know/>. 

In this case though, the EXIF data has been left in the image, as after all, how would we solve the challenge otherwise? 

There are two main ways to view EXIF data depending on what operating system you are using. If you're on windows, the easiest way is to just download the image, right click and view properties. If EXIF data exists in the image, Windows will present that data in the properties:

![Windows Image Properties](/assets/img/sofiasantos-028-properties.png)

If you're using a Linux based OS, you can use [exiftool](https://exiftool.org/) to achieve the same effect: 

![EXIF data from exiftool](/assets/img/sofiasantos-028-exiftool.png)


Whichever method you use, you find yourself to the same answer - the device used to take the photo was - *samsung SM-A125F* which is the model number of a [Samsung Galaxy A12](https://en.wikipedia.org/wiki/Samsung_Galaxy_A12).

In itself this in some way helps to answer the later questions - the photo was taken on a mobile phone rather than any expensive DLSR or other camera. This implies the person taking the photo was travelling, it's probably a quick snap and not something they had time to prepare and compose. 

Note also how the date and time of the photo are also present in the EXIF data and match the detail given in the original challenge. As mentioned before, I suspect this detail was given to allow people not familiar with EXIF data to be able to have a go at the challenge, but anyone who was familiar with EXIF would have been able to figure this out for themselves. 


## Question 2 - Where I was headed?
This question is where things start to get a bit trickier. With just a photo, device model and a date, how can we figure out where someone is going? The key to this question is less technical and more putting together the evidence and doing a bit of google dorking. We know that Sofia has a strong online presence across twitter and LinkedIn. We also know the date the photo was taken. It's not unreasonable to assume that if Sofia was travelling somewhere and taking photos of it on her phone, she might then post about it. 

To check our theory, we need some way of searching her posts around the time the photo was taken. Now of course, we could do this manually, but given how prolific a poster she is, scrolling back to September 2023, almost one year ago, would take a lot of time. 

We can therefore use the power of [google dorking]() to find us the answer. Google is a great search engine. It indexes many many things. Hackers have long since discovered this and used advanced keywords to look for things that should never have been indexed, eg, sensitive directories, admin panels, website misconfigurations. This is commonly called google dorking. For this particular job, we need to find some google keywords that allow us to search around a specific timeframe, for a specific site. 

Enter ```before``` and ```after``` <https://www.searchenginejournal.com/before-after-search-commands/302695/>. These search operators allow you to search google within a particular timeframe. 

We know the post has to have occurred after the photo was taken, so for the ```after``` keyword, we specify ```after:2023-09-20```. We don't know when the post might have been made though, so we can assume any time in the following week, therefore ```before:2023-09-27```. This will search all posts within that timerange. 

However, we only want to search Sofia's posts, not the whole internet, so we need to use the ```site``` keyword. By specifying this, we tell google to search only one specific site. In this case, we can use ```site:twitter.com/Gralhix```, thus making our full search string: ```site:twitter.com/Gralhix after:2023-09-20 before:2023-09-27```

This returns exactly one result:

![Google search result](/assets/img/sofiasantos-028-googleresult.png)

which links to <https://x.com/Gralhix/status/1705952428468605325>:

![Tweet of interest](/assets/img/sofiasantos-028-tweet.png)

So we now know from this tweet that she was headed to the **"SIRIUS CTF Finale: The role of #women in #OSINT investigations"** event. From the rest of the tweet, we learn that this was hosted by Europol. One of the photos on the tweet shows a person wearing a Europol visitors badge, which implies that the person in the picture visiting a Europol building.

The simplest way to find where this building might be is to simply google "Europol", which tells us what the organisation is and where they are based:

![Europol google entry](/assets/img/sofiasantos-028-europol.png)

This tells us the street where the building is based (Eisenhowerlaan) and even the building number (73). We can then verify that this is correct by visiting the area on google maps:

![Europol google maps](/assets/img/sofiasantos-028-europolmap.png)

To categorically answer this question then - Sofia was headed to the Europol building at **78 Eisenhowerlaan, The Netherlands.**

## Question 3 - How far I was from the entrance of my destination?
Now this question is where things get really interesting. We know where Sofia was headed. We know she took a photo on her mobile and that it was early in the morning. From the bonus question, we know she stayed in a hotel. We can therefore make a couple of assumptions. From the time of the photo (08:11), she was headed to Europol. Generally meetings would start around 8:30-09:00, so at 08:11, she can't have been that far away. This is an interesting assumption to keep in mind while attempting to answer this question, however, a fundamental rule of OSINT is that all assumptions should be proven, so we need to find a way to figure out the exact location of the photo. 

At this point we will use one one of the simplest tools available to OSINT analysts - reverse image search. If you've never done this before, the principle is that google and other search engines normally search based on text input. However the modern internet is full of images. Google and others therefore now have functionality to provide an image as the search object, then the search engine will attempt to find matching images. To do this on google, just go to a google home page and press the camera icon to the right of the search box. 

![Google image search](/assets/img/sofiasantos-028-googlesearch.png)

This will ask for an image as input. Once this has been provided, google will find images similar to the one provided:

![Google reverse image search](/assets/img/sofiasantos-028-googleimagesearch.png)

The task of the OSINT analyst now is to analyse each of the images on the right - do they match the input image? Are they in a similar location? We already know that Sofia was in the Netherlands, so we can discount any images from other locations. After examining all of the results, none appear to match our input image. 

Another great feature of google reverse image search is that it allows you to focus on a particular part of the picture to try and match. It does this using the familiar crop style tool, so you can crop the image on the fly to tell google which part of the image you want to match. As OSINT Analysts, this allows us to focus on certain objects or buildings, all of which can be very useful if we feel that particular items or objects in the picture are distinctive and more likely to yield results. 

In this case, perhaps the most distinctive item in the original picture is the building itself, so we focus the search on this area. 

![Google reverse image search cropped](/assets/img/sofiasantos-028-googleimagesearchcropped.png)

Aside from the first result, which is a link back to Sofia's blog (always funny when that happens), the second result immediately springs out because of the address - eisenhowerlaan 59-61. This links back to question 2, where we discovered that the Europol building is at Eisenhowerlaan 74. As such we investigate this image by going over to [flickr](https://www.flickr.com/photos/klaasfotocollectie/3043686035/in/photostream/). 

![Flickr result](/assets/img/sofiasantos-028-flickr.png)

While the building doesn't look exactly the same as the building in the original picture, there are several architectural similarities, including the drains in use, the angular window bays and the type of brick in use. 

The easiest way to prove one way or another however is to head over to Google Maps or Google Earth and check out the building on streetview. By doing this and looking around, we soon realise that although the image found via reverse image search isn't the building we were looking for, the one next door is!

![55 Eisenhowerlaan](/assets/img/sofiasantos-028-eisenhowerlaan.png)

By looking around and comparing to Sofia's original image, we can match various features to confirm that this is the same building - the green drain coming down the side of the building, the green gate with the number 55 on it, the green postbox, the cut off tree stump. All of these match, which proves that this is the same building. 

With that, we can work out the answer to the original question - **"How far I was from the entrance of my destination?""**. This is a straightforward matter of using google maps to work out the distance: 

![Distance from entrance](/assets/img/sofiasantos-028-distance.png)

According to google, the photo was roughly **80m** from the entrance of the final destination of the Europol building. Depending on how precise the answer needs to be, there's probably a little more distance that can be added walking round the building to find the entrance.  

## Bonus Question - In which hotel did I stay?
I'll be completely honest here - I didn't manage to solve this one. I did spend some time trying, but unfortunately I ran out of time and just wanted to get the blog post out. 

In the hopes that this might be useful to some others, I'll list what I tried here:

* Reverse image search on the picture of the challenge coin - this brings up Sofia's LinkedIn post about the same event, where she states that the picture was taken from her hotel balcony
* With that in mind, I spent some time looking for hotels near the Hague that look similar to the hotel you can see in the distance, with three columns and three black objects on the top - unfortunately, no joy. 
* I then spent some time trying to figure out a possible route Sofia could have taken to Europol (we already know at least part of the walking route), in the hope that this would lead back to the initial hotel - again no joy
* As a last ditch attempt, I went to booking.com, typed in hotels in The Hague and looked through the pictures for a hotel matching the description above - again - no joy

I reckon it must be solvable by just reading her other tweets and LinkedIn posts, but I didn't want to cheat by doing it this way. If I get a bit more time, I might come back and have another go, but for now it will have to remain unsolved. 

## Conclusion 

This exercise was a lot of fun!! One of the things I like most about this style of OSINT challenge is getting to see different towns and cities and broadening my geographical horizons. I don't think prior to this I've ever looked at the Netherlands in any greate detail, nor did I know where Europol were based. OSINT truly is a window to the world and I would encourage anyone to have a go if they're interested! 

