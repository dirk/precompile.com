---
layout: post
title:  Smart devices and security
date:   2014-07-08 14:00:00
---

It was recently revealed that LIFX "smart" lightbulbs have a demonstrated vulnerability that allows for remote attackers within range of the bulbs to gain access to the [plaintext passwords of the WiFi network](http://arstechnica.com/security/2014/07/crypto-weakness-in-smart-led-lightbulbs-exposes-wi-fi-passwords/) they're connected too. Bulbs with firmware versions prior to 1.3 (ie. pretty much all of the ones on the market) use a hardcoded AES key to encrypt the mesh network exchange of WiFi passwords among bulbs. As "smart homes" and networked devices become more prevalent we're going to run into more and more of these issues, and it's important to address these sorts of problems now instead of when we're swimming in an ocean of vulnerabilities in every device in our home.

#### Firmware updates are bullshit

The entire idea of firmware is that it's "firm"; it's solid and reliable and doesn't need to be updated every month with security patches. It shouldn't be acceptable to ship a sub-par product to market (and LIFX *shipping hardcoded passwords/keys is definitely sub-par*).

We also come to the problem of how we even deploy firmware updates to smaller and more esoteric devices. Having a USB port or SD card slot on a microwave, lightbulb, or rice cooker seems a bit ridiculous. I wouldn't mind being able to remotely monitor the cooking progress of my brown rice (getting brown rice cooked how I like it is a finicky process), but delivering such functionality should not require a complex enough device to lead to the sorts of developer errors/missteps that require security patches. In the event that updates for fixes to vulnerabilities are necessary, the process for updating should be visible and easy for the user. (For example, the clutziness of updating wireless routers should *not* be the standard for firmware updates.)

However, fully-automatic updates should be avoided. Updates can dramatically change functionality and therefore massively disrupt user experience. If I have a device with known functionality and vulnerabilities, I should have the option to live with said vulnerabilities if the current functionality is critical to me. Automatic updates deny me that option and represent an assault on the user's freedom.

Therefore we need a way to notify users of updates to their and make updating a *simple and transparent* process. The user should never be surprised and/or dismayed after performing an update.

#### Security cannot be secondary

Hardcoding an encryption key like the LIFX developers did is one of the most basic of security no-nos. If something is hardcoded in any sort of on-device storage and that device is in the wild, you can be **damn sure that attackers are going to get that key**. It's what happened with [DVD-CSS](http://en.wikipedia.org/wiki/Content_Scramble_System#Cryptanalysis), [Blu-ray AACS](http://en.wikipedia.org/wiki/Advanced_Access_Content_System#Security), and [HDCP](http://en.wikipedia.org/wiki/High-bandwidth_Digital_Content_Protection#Circumvention), and those all had mountains of money and development behind them, so don't think your device is going to be in any way an exception to the classic hardware attack. If attackers have hardware access and your keys are on the hardware then **you've already lost**.

As we put more and more of these Internet-connected "smart" devices into our homes, workplaces, and lives we need to remain ever-vigilant of their security. Companies must be rigorous in building and testing the security of their devices, to do any less is to expose their customers to myriad risks (in turn exposing the company to liability and litigation).
