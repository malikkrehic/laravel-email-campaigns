---
title: Handling events
weight: 4
---

This package fires out several events. You can listen for them to perform extra logic.

## `Spatie\EmailCampaigns\Events\Subscribed`

This event will be fired as soon as someone subscribes. If [double opt in](https://docs.spatie.be/laravel-email-campaigns/v1/working-with-lists/using-double-opt-in/) is enabled on the email list someone is subscribing to, this event will be fired we the subscription is confirmed.

The event has one public property: `$subscription` which is an instance of `Spatie\EmailCampaigns\Models\Subscription`. 

You can get to the email address of the subscriber like this:

```php
$email = $event->subscription->subcriber->email;
```

This is how you can get to the name of the email list:

```php
$nameOfEmailList = $event->subscription->emailList->name;
```

## `Spatie\EmailCampaigns\Events\Unsubscribed`

This event will be fired as soon as someone unsubscribes. When somebody unsubscribes, the subscription won't be deleted, but its `status` attribute will be set to `unsubscribed`.

The event has one public property: `$subscription` which is an instance of `Spatie\EmailCampaigns\Models\Subscription`. 

You can get to the email address of the subscriber like this:

```php
$email = $event->subscription->subcriber->email;
```

This is how you can get to the name of the email list:

```php
$nameOfEmailList = $event->subscription->emailList->name;
```

## `Spatie\EmailCampaigns\Events\CampaignSent`

This event will be fired after you've sent a campaign, and all mails are queued to be sent. Keep in mind that not all your subscribers will have gotten your mail when this event is fired.

The event has one public property: `$campaign`, which is an instance of the `\Spatie\EmailCampaigns\Models\Campaign` model.

## Spatie `Spatie\EmailCampaigns\Events\CampaignMailSent`

This event will be fired when a mail has actually been sent to a single subscriber.

It event has one public property: `$campaignSend` which it and instance of the `Spatie\EmailCampaigns\Models\CampaignSend` model. 

You can get to the email the mail was sent to like this:

```php
$email = $event->campaignSend->subscription->subscriber->email;
```

You can also retrieve the campaign name this mail was sent for:

```php
$campaignName = $event->campaignSend->campaign->name;
```

## `Spatie\EmailCampaigns\Events\CampaignOpened`

This event will be fired when somebody opens an email. Be aware that this event could get fired many times right after sending a campaign to a email list with a large number of subscribers. This event will only be fired for campaigns that have [open tracking](https://docs.spatie.be/laravel-email-campaigns/v1/working-with-campaigns/tracking-opens/) enabled.

It has one public property: `$campaignOpen`, which is an instance of the `Spatie\EmailCampaigns\Models\CampaignOpen` model.

You can get to the email address that opened your email like this:

```
$email = $event->campaignOpen->subscriber->email;
```

This is how you can get to the name of the campaign:

```php
$name = $event->campaignOpen->campaign->name;
```

## `Spatie\EmailCampaigns\Events\CampaignLinkClicked`

This event will be fired when somebody clicks a link in a mail. This event will only be fired for campaigns that have [click tracking](https://docs.spatie.be/laravel-email-campaigns/v1/working-with-campaigns/tracking-clicks/) enabled. 

It has one public property `$campaignClick`, which is an instance of the `Spatie\EmailCampaigns\Models\CampaignClick` model.

You can get to the url of the link clicked like this:

```php
$url = $event->campaignClick->link->original_link;
```

The email address of the subscribed who clicked the link can be retrieved like this:

```php
$email = $event->campaignClick->subscriber->email;
```

## `Spatie\EmailCampaigns\Events\CampaignStatisticsCalculated`

After a campaign has been sent, statistics will frequently [be calculated](https://docs.spatie.be/laravel-email-campaigns/v1/working-with-campaigns/viewing-statistics-of-a-sent-campaign/).

Each time the statistics are calculated this event will be fired. It has one public property `$campaign`, which is an instance of the `Spatie\EmailCampaigns\Models\Campaign` model.
