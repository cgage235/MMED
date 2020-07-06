

## Running Locally

mmed is built on React Native and therefore assumes you have [node](https://nodejs.org/en/) installed.
[Yarn](https://yarnpkg.com/en/) is preferred over NPM as a package manager.

```sh
# clone the project and cd into it
git clone git@github.com:Flaque/quirk.git; cd ./quirk

# copy the sample .env (edit as required)
cp .env.sample .env

# install dependencies
yarn

# start development environment
yarn start
```

You'll then be in the [expo development environment](https://docs.expo.io/versions/latest/).
If you already have XCode installed with a simulator, you can just press `i` to start it.


Of course!

**If you're a mental health professional,** audit [the descriptions](https://github.com/Flaque/quirk/blob/master/src/locals/en.json) of the cognitive distortions. If you have suggestions, let me know and we'll change stuff!


**Don't include features for one particular condition at the expense of other conditions.** For example, don't couple mood tracking to thought tracking. If a user _has_ to enter a mood in order to track a thought, then the entire app is ruined for people who use it for panic, OCD or another condition where mood isn't the primary focus.

**Don't include non-CBT related treatments without good reason.** No relaxation audio tracks or meditation guides. It's a CBT app, keep it focused on CBT.

**Don't include things that could be better accomplished by another app.** No one needs an in-app diary when a diary works just fine. No one needs an in-app heart rate tracker when a heart rate tracker works just fine.

**Be quick and efficient.** Thoughts shouldn't take 5 minutes to enter and you should be able to skip fields if it's reasonable. Don't let the perfect be the enemy of the good.

**Thoughts are more valuable than passwords, treat them that way.** Most people would rather give over their passwords than their CBT thoughts. They're incredibly private, occasionally involve other people, and frequently are embarrassing.

**Don't have \$200 dollar in app purchases.** I'm looking at you CBT Thought Diary. I get it, developers need to make money. It costs a lot to just keep the app on the app store. But you're preying on vulnerable people. Very few people of rational mind will purposely spend \$200s for a dark mode.

**Don't have dumb notifications.** Scheduling is fine, abusing push notifications so your app has better traffic is scummy and gross.

**Be open.** Not every app has to be open source; it's a hard choice to make. But be clear and obvious within the app about what's going on with the user's data. Don't be sending it to some server without making that clear within the app, especially if it's not providing any extra utility to the user.

**Don't push people to be unhappy.** Do not purposefully or accidentally force people to be unhappy to use their app. Don't force people to state their unhappy in order to access a feature. It's easy for this to sneak up in the design, if a user has to rate their happiness below average in order to access the CBT features, you're asking them to be unhappy to use your app.

**Be extremely cautious about making engagement your core metric.** User engagement is fine to be concerned about. We all want people who need help to be actually engaging in the help. But holy moly becareful about this. You _do not_ want to drive something that is for many people a treatment into a self-perpetuating engagement loop. A ruthless focus on engagement has caused many a product to become skinner boxes. _No one should ever be addicted to your mental health app._

# Engineering Logic

Quirk _must not_ lose user data. The entire point of the app is to record your thoughts, so if you lost them it would be pretty bad. As stated in [one study](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6010839/):

> While an app failure in general can be inconvenient and annoying, it can have serious consequences in the context of mental health apps—someone who has come to rely on an app for emotional support can find a failure “devastating.”

Therefore, data management should be given a higher priority than any other part of the app.

## Taxonomy and Order of Data Failure Cases

The following is a list of extremely _bad_ behaviors and states that could happen in order of severity.

### 1 - Large Scale Data Corruption

All thoughts have been corrupted somehow. For example, the JSON format of every item is wrong. This is put at the top because not only can a user not access the data, but it may spiral out can cause continuing errors forcing the app to be "bricked."

### 2 - Large Scale Data Loss

All thoughts have been deleted without any hope of recovery.

### 3 - Small Scale Data Loss

A small amount of data has been deleted without any hope of recovery.

### 4 - Small Scale Data Corruption

A small amount of data has been corrupted in a recoverable way. The user still has lost data, but the app does not crash, and this is potentially fixable via an update.
