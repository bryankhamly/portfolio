---
title: "CI/CD for Team73"
featureImage: images/cicd.png
tags:
- Development
categories:
- Development
---

# What’s the CI/CD pipeline?

This stands for Continuous Integration and Continuous deployment.

For Team73, we are using TeamCity and AWS for our tech stack.

# How does it work?
The TeamCity server is hosted on my trusty laptop right next to me. It watches for new changesets on the PlasticSCM repo hosted on a cloud.

When a new changeset is detected it starts an automated build, notifies discord and then uploads it to our AWS bucket. Once its uploaded, we execute a python script to notify discord for the correct download link.
```python
import sys
from discord_webhook import DiscordWebhook, DiscordEmbed

webhook = DiscordWebhook(url='https://discord.com/api/webhooks/997986909450145843/MKuZTnv37Lr8Dkyhi0qSAo-TBSACWHoOuQmFg7dP-l2kCU9CRVIx0n-nztY61FovHZ8k')

buildNumber = sys.argv[1]
downloadLink = "https://team73.s3.us-west-1.amazonaws.com/SurvivalXD/SurvivalXD_SurvivalXDBuildConfig/{}/Game.zip".format(buildNumber)

embed = DiscordEmbed(title='Team73 - Build Uploaded', description=downloadLink, color='03b2f8')

webhook.add_embed(embed)

embed.set_thumbnail(url='https://i.imgur.com/IM2LTsG.png')
embed.set_footer(text='Have a good day!')
embed.set_timestamp()

response = webhook.execute()
```
![cicd2.png](/images/cicd2.png)