---
layout: page
title: "The State of IMSI Catching Catching"
#
# Content
#
author: freddy_martinez
teaser: "Recently, the Lucy Parsons Labs was sent an email about detecting IMSI catchers, often called Stingrays, at protests. Our experience detecting IMSI catchers in the last year is below."
categories:
  - stingray
tags:
  - technology
  - surveillance
#
# Styling
#
header: no
image:
 thumb: "thumbs/StingrayThumb.jpg"
---

This is an area with which I have a good bit of experience. I have spoken about on the topic at conferences including [DEF CON](https://www.youtube.com/embed/JyTb5mJOYL), [THOTCON](https://github.com/freddymartinez9/securitytalks/tree/master/Thotcon) and wrote a [guide for protests.](https://github.com/freddymartinez9/securitytalks/blob/master/IMSICatchersForActivists.md). The Lucy Parsons Labs has spent over a year seeking evidence for IMSI Catcher use at Chicag-area demonstrations. 
While our research has found roughly one interesting case, over the last year have found no confirmed hits of IMSI catchers at protests in Chicago. However our methodology faces critical technical limitations, in both hardware and software. (Note: In this blog post, the quoted text is from the email we received seeking our help). 


> There's a strong feeling in the activist communities around the country there is some sort of cell phone surveillance going on during protests, that is possibly being used to monitor/disrupt communication and identify protest leaders, but no one has yet been able to prove it in a solid manner."

The Lucy Parsons Labs work has not captured a confirmed IMSI catcher being used _in Chicago-area_ protests but there is significant evidence that Law Enforcement Officials (LEOs) are using IMSI catchers to surveil activists at demonstrations. For example, there is at least one purchase order for a Stingray in [Florida in 2004](http://www.documentcloud.org/documents/2169898-miami-dade-police-stingray-purchase-to-target.html) around the FTAA/anti-capitalism protests. Photographic evidence exists for a deployment in Dresden Germany and a deployment during unrest at [Taksim Square](https://gitlab.com/Hounge/Android-IMSI-Catcher-Detector) in Istanbul, Turkey [picture](http://i43.tinypic.com/2i9i0kk.jpg). 

![Dresden](/images/blogimages/DresdenIMSICatcher.jpg)

There also seems to be evidence of a fake tower emerging in Kiev where SMS were sent to protests (below) as reported in the [New York Times](http://thelede.blogs.nytimes.com//2014/01/22/ominous-text-message-sent-to-protesters-in-kiev-sends-chills-around-the-internet/).  This could have easily been done with an IMSI Catcher, and as an added benefit of capturing phone numbers, SMS messages could be sent to suspected protesters for free. 

![Kiev](/images/blogimages/Kiev.jpg)

It is, however, unclear how often this form of surveillance is happening in a protest context. (IMSI catchers are widely used otherwise in the United States for criminal investigations, often without meaningful oversight). While LEO use of an IMSI catcher would not make it possible to identify individual protest leaders, the practice would nonetheless allow a government a window into the social networks among protesters, an associational mapping that is both constitutionally fraught and a valuable commodity. 

Therefore, IMSI catcher use at protest is a realistic risk that one should incorporate into their threat model. 

> We have explored several avenues of getting this done, but have not found a way that we believe will reliably detect an attack by an IMSI-catcher device, and that will also generate some sort of report that we can use as proof.

Our efforts to unmask an IMSI catcher by technical means have taken several avenues, none of which has yet been able to both reliably detect an attack by an IMSI-catcher device and that also generate some sort of report that would serve as sufficient “proof.” The best evidence I have analyzed to date of a "capture" is the packet captures (pcaps) that LPL reviewed in [collaboration with NorthStar Post](http://nstarpost.com/17486/159855/a/cellphone-surveillance-used-on-black-lives-matter-protesters-at-fourth-precinct). The pcaps can be download directly from [this link](https://s3.amazonaws.com/nstarpost-public/imsicatcher-minn/snoopsnitch_2015-11-25_21-39-25UTC.pcap) There is a request for an IMSI rebroadcast which is suspicious, however that alone is not proof of an IMSI Catcher. For further urther evidence of an IMSI Catcher, an analysis might show the following: 

 * a new LAC appearing when it didn't appear.
 * a new CID or an empty CID that didn't exist before it.
 * broadcasting on new ARFCN
 * Long delays in ACK messags from the BTS 

With a given pcap, you would then do a calculate of all the network metrics and subscribe a score to how suspicious particular events are. (This methodology is in fact what SnoopSnitch does, we will talk about that a bit further).  In the case of the NorthStar Post pcaps, my analysis came out to medium. To me, it wasn't proof positive, although it is possible this data was in fact generated by an IMSI catcher.  

There are several options for capturing information. There is Cryptophone, a fancy name for a modified Samsung Galaxy S3 which has a buggy baseband that allows direct access to it. Because a Cryptophone has direct access to the most interesting levels of the baseband OS, which is where the information we are interested in resides, it is a potential tool for countersurveillance. It is, however, expensive and while I have seen their product a few times, it doesn't strike me accessible to most people for the price point. 

In addition to a stand-alone hardware solution, there are two software solutions "SnoopSnitch" and "AIMSICD", both of which work on Android phones. SnoopSnitch requires root on your phone, which is needed since you need baseband access and is essential for IMSI Catcher detection. 

> If you were in our shoes, and attempting to catch IMSI-Catcher activity at a protests, what would you use?

The Lucy Parsons Labs, and our team of volunteers, run rooted Moto Es with SnoopSnitch. Once we have captured data, we can export pcaps off SnoopSnitch for further analysis into WireShark using their GSM filters. We have run the Moto Es with SnoopSnitch for over a year and have gotten no hits at protests. It is a bit frustrating but its unrealistic to pull out one phone at one protest and expect a hit. More surprisingly is when we _didn't_ get hits, namely when POTUS was in town and near us, as was expected since he travels with an [IMSI Catcher](https://twitter.com/csoghoian/status/613110943514374146) and likely an IMSI Catcher Catcher. Based on our review of SnoopSnitch, it seems to be lacking support for 4G/LTE which it appears the team behind SnoopSnitch is working on. So this form of countersurveillance is a work-in-progress and our methodology needs further development.

That doesn't mean I don't recommend running the SnoopSnitch on rooted Moto Es. Our best guess right now is to do a lot of reconnaissance and counter surveillance, which is time consuming because it must be done before and during demonstrations. We do need more of these out all over the place, however I think it is important to be rigorous in your methodology and be reasonable with expectations.

See you at the RNC. 
