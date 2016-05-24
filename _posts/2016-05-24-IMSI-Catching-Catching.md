---
layout: page
title: "The State of IMSI Catching Catching"
#
# Content
#
author: freddy_martinez
teaser: "Recently, the Lucy Parsons Labs was sent an email about detecting IMSI catchers at protests. We hope to explain our current state of detecting IMSI Catchers based on our experiences in the last year."
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

This is an area which I have spoken about multiple times including at [DEF CON](https://www.youtube.com/embed/JyTb5mJOYL), [THOTCON](https://github.com/freddymartinez9/securitytalks/tree/master/Thotcon) and even wrote a [guide for protests](https://github.com/freddymartinez9/securitytalks/blob/master/IMSICatchersForActivists.md). We have roughly found one interesting case, which we did an analysis on, but over the last year have found no confirmed hits of IMSI catchers at protests. However, there are technical limitations in our methodology, namely that SnoopSnitch hasn't functioned as we expected. In this blog post, the quoted text is from the email we received. 


> There's a strong feeling in the activist communities around the country there is some sort of cell phone surveillance going on during protests, that is possibly being used to monitor/disrupt communication and identify protest leaders, but no one has yet been able to prove it in a solid manner."

![Kiev](/images/blogimages/Kiev.jpg)

It is a **fact** that LEOs are using IMSI Catchers to surveil activists at protests. There is at least one purchase order for purchasing a Stingray _before_ the FTAA protests in [Florida in 2004](http://www.documentcloud.org/documents/2169898-miami-dade-police-stingray-purchase-to-target.html). There seems to be a deployment in Dresden (below) and another one at [Taksim Square: ](https://gitlab.com/Hounge/Android-IMSI-Catcher-Detector) [picture](http://i43.tinypic.com/2i9i0kk.jpg). There also seems to be evidence of this in Kiev where SMS were sent to protests (above) as reported in the [New York Times](http://thelede.blogs.nytimes.com//2014/01/22/ominous-text-message-sent-to-protesters-in-kiev-sends-chills-around-the-internet/).  This could have only been done with an IMSI Catcher. However, it is unclear how _often_ this is happening and in what capacity. It would not be possible to identify protest leaders this way, however IMSI catching at a protest would give you a sense of the social network at the protest. The social network could then be mapped; this is a realistic risk that one _should incorporate_ in their threat models. 

![Dresden](/images/blogimages/DresdenIMSICatcher.jpg)

> We have explored several avenues of getting this done, but have not found a way that we believe will reliably detect an attack by an IMSI-catcher device, and that will also generate some sort of report that we can use as proof.

There doesn't seem to be a reliable way to do this, at least in a way I would recommend as "proof". The best evidence I have seen for a "capture" is the pcaps that LPL has reviewed in [collaboration with NorthStar Post](http://nstarpost.com/17486/159855/a/cellphone-surveillance-used-on-black-lives-matter-protesters-at-fourth-precinct). The pcaps can be download directly from [this link](https://s3.amazonaws.com/nstarpost-public/imsicatcher-minn/snoopsnitch_2015-11-25_21-39-25UTC.pcap) There is a request for an IMSI rebroadcast which is suspicious, however that alone is not proof of an IMSI Catcher (to me). For further evidence of an IMSI Catcher, you need to see to see the following in your pcaps: 

 * a new LAC appearing when it didn't appear.
 * a new CID or an empty CID that didn't exist before it.
 * broadcastin on new ARFCN
 * a call to the GetNeighboringCell changes and compare those changes. A GetNeighboringCell looks like this (roughly) ```[<MCC>,<MNC>,<LAC>,<CI>,<BSIC>,<BCCH Freq>,<RxLev>... ]```

With that information, you would maybe calculate some weighed averages and subscribe a score to how suspicious the event was.  In the case of the NorthStar Post pcaps, my analysis came out to medium, it certainly wasn't a slam dunk event. I might be wrong but we need much more information or better captures. 

There are several options for capturing information. There is Cryptophone, a fancy name for a modified S3 with a buggy baseband that allows direct access to it. It does allow direct access to the most interest levels of the baseband OS which is where the information we are interested in resides. It's expensive and while I have seen their product a few times, it doesn't strike me accessible to most people for the price point.  There are two software solutions, "SnoopSnitch" and "AIMSICD". Both work on Android phones but SnoopSnitch requires root on your phone, which as described above is the way to go since you need baseband access. It is true that the US hasn't been extensively mapped using SnoopSnitch, however before you go to a protest, you should know the layout of current BTS (cellphone towers) topography ahead of time, as you do your reconnaissance for countersurveillance. 

> If you were in our shoes, and attempting to catch IMSI-Catcher activity at a protests, what would you use?

We run rooted Moto Es with SnoopSnitch. You can also export pcaps off SnoopSnitch for further analysis into WireShark using their GSM filters. We have run the Moto Es with SnoopSnitch for over a year and have gotten no hits at protests. It is a bit frustrating, but its unrealistic to pull out one phone at one protest and expect a hit. More surprisingly is when we _didn't_ get hits, namely when POTUS was in town and near us, as was expected since he travels with an [IMSI Catcher](https://twitter.com/csoghoian/status/613110943514374146) and likely an IMSI Catcher Catcher. Based on our review of SnoopSnitch, it seems to be lacking support for 4G/LTE which it appears SRLabs is working on. It's a work in progress and it appears that our methodology needs further development or that we need better detection methods.  

That doesn't mean I don't recommend running the SnoopSnitch on rooted Moto Es. Our best guess right now is to do a lot of reconnaissance and counter surveillance, which is time consuming because it must be done before and during demonstrations. We do need more of these out all over the place, however I think it is important to be rigorous in your methodology and be reasonable with expectations.  

