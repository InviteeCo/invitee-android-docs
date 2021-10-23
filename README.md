# Setting up the Android SDK
Follow the steps below to setup the invitee sdk in your existing project.


### Install SDK

Install the invitee sdk

#### build.gradle (module)
```markdown
implementation "com.gitlab.invitee:invitee.sdk.android:1.0.0"
```

#### gradle.properties
```markdown
authToken=jp_966vc54dg6grckmod53mp3h9i6
```

#### build.gradle (project)
```markdown
allprojects {
    repositories {
        ...
        maven {
            url 'https://jitpack.io'
            credentials { username authToken }
        }
    }
}
```

#### invitee.xml
Create the invitee config file in /res/values (ie. res/values/invitee.xml)
```markdown
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <string name="co_invitee_sdk_api_key">YOUR API KEY HERE</string>
</resources>
```

Ensure app allows access to contacts.
If you haven't generated an api key before, you can create from within the [web dashboard](https://app.invitee.co/account/api-keys)


### Setup a user
Calling setup user is the first step. You should call this as soon as a user is logged in. This will add a user to a campaign if one is available & fetch information about the campaign, which you can then show in your UI

```markdown
val inviteeUser = User("123456", "test", "user", "0466598683")

InviteeSDK.setup(inviteeUser, object: InviteeSetupCallback {
    override fun campaignAvailable(campaign: Campaign) {
        // show the referral information & a CTA to present the campaign overview page
    }

    override fun noCampaignAvailable() {
        // no live campaigns or all campaigns are full
    }

    override fun error(exception: InviteeSDKException) {
        // an error occurred
    }
})
```

### Present campaign overview page

The campaign overview page shows information to a user about the current referral campaign they are in & allows them to send invites to their friends.
The information seen on this page reflects what you have setup for that campaign in the web dashboard.

```markdown
InviteeSDK.present(requireActivity())
```

### Present referral notifications

When users signup both the referrer & referee can be notified about who of their friends signed up & what reward they earned.
Call this on a UIViewController where you want to present these popups.

```markdown
InviteeSDK.presentNotificationIfNeeded(requireActivity())
```

### Tracking referral steps

In order for Invitee to process referrals, you need to let us know when key 'steps' have been completed. These steps are configured when you setup a campaign & are used to indicate a completed referral. Eg. Sign up + make purchase = referral reward.

There are two ways to track these steps, either via our REST api or through the SDK itself. If you choose to use the SDK then the following code snippet can be used as a reference for tracking.

```markdown
InviteeSDK.trackReferralStep(user, "SIGNUP", object: InviteeTrackCallback {
    override fun success() {
        // successfully tracked a referral step
    }

    override fun error(ex: InviteeSDKException) {
        // an error occured
    }
})
```
