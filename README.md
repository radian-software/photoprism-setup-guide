## PhotoPrism setup guide

[PhotoPrism](https://www.photoprism.app/) is a self-hosted, free and
open-source photo and video library manager, which can replace
services such as [Google
Photos](https://en.wikipedia.org/wiki/Google_Photos) or [Apple
Photos](https://en.wikipedia.org/wiki/Photos_(Apple)). It requires a
bit more leg work than those other services to set up, so I have put
together this short guide about how I recommend getting started.

When I did my migration, I set up PhotoPrism to work alongside Google
Photos so that my photos were available in both services at the same
time, and new photos were uploaded to both. That way I could evaluate
the services comparatively without committing either way. After I felt
good that all my data had been copied over and was accurate, I stopped
using Google Photos, and eventually deleted my account data.

### Server setup

First you have to decide where you want to run PhotoPrism. Unlike a
commercial cloud service, your photos (and all the software managing
them) will live on infrastructure that you own. However, these days it
is pretty easy to rent servers online and not have to deal with the
inconvenience of server administration.

There is an even simpler option which is to select a fully managed
server rental. I decided to go with
[PikaPods](https://www.pikapods.com/) because they are [officially
recommended by
PhotoPrism](https://docs.photoprism.app/getting-started/cloud/pikapods/).
You just sign up for an account, ask for a server running PhotoPrism,
and they give you one. You don't have to do any installation or
software updates.

#### PikaPods options

PikaPods bills hourly, so you only pay for what you use, based on the
resources needed by your server. The minimum for PhotoPrism is 2 cores
CPU, 8 GB memory, 10 GB storage, for $6.50/month. This is quite cheap
as far as servers go so I am pretty happy with it. You can increase
the storage later if you need, without any disruption. I personally
pay for 150 GB storage, for an extra $1.80/month or so.

You probably want to start by creating a server with the minimum
required resources, get things set up, then increase the resources
later and add a credit card with autopay if you decide you like it.
Don't forget two-factor authentication, that is good to enable on
every website, especially ones where you can get billed for people
buying things on your account.

Most of the settings in PhotoPrism you can configure through the UI,
there are just a few low-level things that are configured through
PikaPods via "environment variables". Mostly you need to set your
initial password in the `PHOTOPRISM_ADMIN_PASSWORD` field when
creating your server. Generate a random password and put it in your
password manager. You'll login with that password initially and can
change it later if you want.

One thing to note with PikaPods is after creating the server, it seems
you can only change one setting at a time. You have to wait a few
minutes after saving changes before you can make more changes. If you
get an error about that, try waiting a bit and refreshing the page,
then making the change again that you were trying to make.

#### Domain name

By default you'll get a randomly generated domain name for your
PhotoPrism server, `something.pikapod.net`. You can change the
`something` if you want. Or if you have your own website, you can use
that. For example I have a personal website at
<https://intuitiveexplanations.com>, so I set up my PhotoPrism server
to be located at <https://photos.intuitiveexplanations.com>. That way
if I ever decide to move my server to another provider instead of
PikaPods, I can keep using the same domain name.

If you want to set a custom domain you can configure that in the
settings. You just have to have access to create DNS records for your
website following the instructions in the settings UI. There are
guides for this from PikaPods and from other providers because this is
a common task. The main thing you have to know is who you are actually
renting your domain name from, because that is where you will have to
go to manage DNS records.

It'll take a few minutes after configuring a custom domain before
things start working, because DNS records take time to propagate and
then PikaPods has to provision a TLS certificate which can take a bit
of time. If it is still not working after an hour something is
probably wrong.

**Gotcha:** The interface for PikaPods configuring a custom domain is
a little weird. When you enable the checkbox for using a custom
domain, the option for changing the `something` in
`something.pikapod.net` goes away, which makes you think it is no
longer relevant. Actually, it is still relevant, just in a less
obvious way. It's used internally for some things like the
administrative database interface, so you will still see it from time
to time. So what I recommend is first changing the `something` to a
value that makes sense, like `yourname-photos`, saving your changes,
waiting a few minutes (see above), and then enabling custom domain and
following the directions for that.

### Initial configuration

Once you have your PhotoPrism domain name set up you should login
using the username `admin` and the password you entered when creating
the server. You probably want to spend some time clicking around the
UI and seeing how things are laid out.

Things you might want to adjust in the settings page in PhotoPrism:

* General > User Interface - what features are enabled. I turned on
  `Delete` to make it possible to permanently delete photos, and
  disabled `Places` and `Moments` because I don't care about those
  things particularly.
* Library > Index - the Quality Filter option is a bit surprising if
  you are not expecting it. I turned this off because I found that
  many of my photos were not showing up after I imported them, which
  turned out to be because they had been flagged as "low quality"
  (lol). There's documentation
  [here](https://docs.photoprism.app/user-guide/organize/review/)
  about the quality filter, maybe it will work better for you.
* Advanced > Global Options - if you run into inexplicable errors when
  trying to perform an operation then it can be useful to go to
  PikaPods and check the server logs, which you can do from the server
  settings page. Oftentimes if an operation shows an error message
  then there will be more information printed in the logs about what
  happened. If you aren't getting anything useful from there, you can
  turn on Debug Logs and maybe get even more info.
* Account - you can upload a profile picture and set your name if you
  want. Since PhotoPrism is self-hosted nobody will see any of this
  except you, but maybe you just like filling out form fields.
* Settings > Users - you can create user accounts for other people on
  your PhotoPrism, so they can e.g. view/edit all your photos even
  without you sharing them. Maybe if you are using PhotoPrism as a
  family photo album this would be relevant.

Things to be aware of about the UI that I didn't realize at first:

* Keep a look out for the little triangle arrow icons in the sidebar.
  Those are clickable and expand more menu options which I didn't
  realize existed at first. For example if you want to visit the
  Archive, that's nested under the triangle icon next to Search.
* The "main view" that you would be familiar with from Google/Apple
  Photos is called Search. Before you search for anything, it just
  shows all your photos chronologically.
* The Library page isn't really for viewing your library like you
  might expect. It's more like an operational dashboard which is where
  you go if you want to import new photos or tell PhotoPrism to
  re-scan your library.

### Importing photos

There are a couple of ways to get photos into PhotoPrism. It might be
a good idea to import a couple photos manually just to see what they
look like in the interface. The way you do this is in Library > Import
and selecting files to upload.

PhotoPrism uses two main folders, "imports" and "originals". The
simple workflow is you put new photos into "imports" and they
automatically get sorted and categorized, and then placed in a nice
subdirectory in "originals" where they live in your library. But if
you already know how you want to organize your library, you can
manually put photos in "originals" and PhotoPrism will use them as is.

I personally have been sticking to the flow where you just drop stuff
in "imports" which is also what happens when you upload photos through
the web UI. I selected the "Move files" option on the import page.

After you imported photos, they should show up in the Search page. You
can click on them for details, and if you pick "edit" you can see the
imported metadata, such as the date and time for the photo.

#### Deleting photos

You may want to get rid of the test photos you uploaded. This is a
little less intuitive than I expected. You have to select them and
"Archive" using the button at the bottom right, first, then go into
the Archive menu (which is nested underneath Search), select them
there, and select to delete them. (This option is only available if
you've turned on the Delete feature in settings.)

### Automatically adding new photos

PhotoPrism doesn't have an official mobile app. Instead they have
designed the main web interface to be usable on mobile (you can use
the "put this website as an icon on the home screen" functionality
that is in both iOS and Android these days).

What this means is that, unlike Google/Apple Photos, you have to take
care of uploading photos from your phone on your own. It would be a
pain to have to do this manually, so there is an app called
[PhotoSync](https://www.photosync-app.com/home) which does this for
you. It's a couple of bucks and is what is [officially recommended by
PhotoPrism](https://docs.photoprism.app/user-guide/sync/mobile-devices/).
They support both iOS and Android although I am using the app on
Android.

(Note that PhotoPrism is free and open-source, PhotoSync is paid. And
what you are paying for with PhotoPrism is just renting a server to
run it, you could run it for free on a desktop sitting in your house
if you wanted.)

#### Buying the app

If you are a normal person you can just buy PhotoSync through the iOS
Store or Google Play Store. I personally go to great lengths to avoid
supporting either Apple or Google, so I shot their support address a
mail asking if I could purchase directly. Turns out yes, the CEO very
kindly wrote back saying I could download a pre-release version from
their Dropbox and send them some money to the company PayPal, and they
would assign a license to the unique ID of my installation. So if you
want to go to that effort you can. (They also said that in the future
they would be building another way to do this direct purchase without
having to manually exchange a bunch of emails.)

There is also a 1-week free trial so you can get things started to see
if they work before buying.

#### Initial app setup

The first thing you want to do is grant permissions on your files and
verify you can see your camera roll photos in the PhotoSync app.

Next you want to go to the settings page and "Transfer Targets >
Configure". This is where you plug in your PhotoPrism server (your
domain name, `admin`, and password). They have an explicit option for
PhotoPrism, or you can just use the generic WebDAV option, they both
work the same. [This documentation
page](https://docs.photoprism.app/user-guide/sync/mobile-devices/) has
instructions on it.

The main thing that I found different from what the docs said was that
it seems like PhotoSync already knows it is supposed to upload photos
into the `import` folder (see above). So you don't actually need to
type in `/import/` like the guide says, and in fact when I tried, it
ended up failing to upload photos, and the PhotoPrism server logs said
it was erroring out because it was trying to upload to
`/import/import/`. It seems like `/import` always gets stuck on at the
beginning, so the destination folder that worked for me was just `/`
or the default option.

You can also set the filenames that PhotoSync will use when uploading.
I'm not sure if this makes a difference but the PhotoPrism
documentation
[says](https://docs.photoprism.app/user-guide/library/metadata/#enrichment)
it's possible for timestamps to be read from filenames, so I figured
it couldn't hurt to set the Upload Settings > File Names format to
`Recording Time + Name`, just in case the metadata on a photo was ever
broken for some reason, at least there would be a better chance of
things working out.

Other relevant settings I set:

* Delete After Transfer - No, because I want to keep recent photos on
  my camera roll so they can be viewed without internet, as well as be
  able to have them also upload to Google Photos in parallel, to
  evaluate the two systems together.
* Cellular - No Photo, no video, because it should be good enough to
  wait for wifi for any uploads (Google Photos does this too, by
  default). But this setting turns out to not really matter too much,
  see the discussion on Autosync later.

#### Picking what to upload

PhotoSync has a couple different options for how and when and what you
upload. One thing you have to get comfortable with is that unlike
Google/Apple Photos, PhotoSync is a generic system for copying photos
from one place to another; it doesn't make the assumption that it has
access to your whole library at once. So the place you go for your
whole library is really PhotoPrism; PhotoSync is just a tool for
copying new photos into there from your camera roll. Because of this
there is a bit more complexity about when it makes sense to copy
photos.

One thing you notice is blue borders around photos. These indicate the
photos that haven't been uploaded - or at least that PhotoSync doesn't
*think* have been uploaded. It can't really tell, because as soon as
it uploads a photo, PhotoPrism moves it out of the `imports` folder
(if you have the "Move files" option enabled like I do). So it keeps
track of that on its own.

You have two options for getting started, either you can have
PhotoSync upload everything on your camera roll to begin, or you can
select everything and mark it as already uploaded, so only newly taken
photos will go into PhotoPrism.

(I took the second option, but then I also exported my Google Photos
library using Google Takeout and imported it directly, because
uploading 30 GB of photos would have taken forever. See the section
later on library import.)

#### Making sure it works

Once you know what's up, you can trigger a sync manually on one or two
photos directly from the image browser in PhotoSync to make sure the
actual upload process is working, which is probably a good idea. I
wouldn't recommend trying to manually sync all photos, because it
seems like you can't have it do that in the background, it blocks
other usage of the app during the upload process. I guess that's
because why not use Autosync instead (see below).

Assuming PhotoSync says the upload process worked for your test
photos, you should also check PhotoPrism and see if it has imported
them (you can see the progress on the import page) and then indexed
them (on a different tab next to the import page). They should end up
in the Search view.

It's a good idea to double check that metadata got included properly,
so for example the photos sort in the right order chronologically and
when you go to edit, all the normal metadata is there. I wasn't having
that happen, and it turned out to be because my camera app had an
option to strip EXIF data from photos, which was enabled by default.
So the metadata was just missing from the photos in my camera roll. I
don't know how Google Photos was getting the metadata before, maybe it
was just guessing, or spying on me some other way.

#### Autosync

Once you've figured out what initial photos you want to sync, you can
get autosync set up. This is in the settings menu. You have to pick
the conditions when you want photos to be uploaded. These were a
little fiddly for me to test if they were working properly. It seems
like if you enable an autosync option, it doesn't necessarily start
happening right away. But I turned on the option to do uploads in the
middle of the night while my phone is charging, and the next morning,
it indeed had done that, and it has basically been working as
advertised since then. I would configure what makes sense to you and
then check back in a few days to see if it's working.

I guess the main alternative would be to configure upload to happen on
wifi after each time you take a photo, which is basically what Google
Photos does. I guess it would take more battery to do uploads all the
time though, and I figured I didn't really need that, so I went with
the nightly upload because that seemed more recommended.

### Life in PhotoPrism

I'm still getting started, relatively speaking, with PhotoPrism. It
seems like it is missing a feature or two here and there, but the
development community seems pretty active and there are good
workarounds for a lot of the issues if you are okay with a little
inconvenience as the cost of seceding from Google's
surveillance-capitalism hegemony.

#### Usage on mobile

One disadvantage is there isn't a native mobile app for PhotoPrism,
like with Google/Apple photos. This feels pretty reasonable to me
actually, because developing mobile apps takes a ton of effort and
it's nice they are prioritizing more important things. What you do
instead is just go to your PhotoPrism domain in your mobile web
browser and click the button to install it as an icon on your home
screen. It loads pretty fast and works well on mobile. Obviously it
only works while you're online though, so I think it's a good idea to
set PhotoSync to leave your photos in your camera roll after upload,
so you can still see recently taken photos offline. You just can't
search them.

An alternative to using the progressive web app (PWA), meaning
"install website on home screen", is that some browsers seem to limit
the functionality you have access to when viewing a website in this
manner. For example with Firefox on Android I haven't been able to
figure out how to copy image URLs in this mode (see [question
here](https://support.mozilla.org/en-US/questions/1431859)). So
instead I stopped using it that way, and instead when I want to use
PhotoPrism on Android, I simply go to it in my normal browser. You
could create a bookmark for it.

Another option is to try the [unofficial Android
app](https://github.com/Radiokot/photoprism-android-client). It
doesn't have all the features of the official web version of
PhotoPrism but it is more convenient sometimes, so I also have that
installed personally. I am not sure what the options are on iOS.

#### Sharing photos/albums

If you want to share an individual photo you can just view it in your
library, right-click, and copy the image URL. The image URL has a
secret token in it that will let people view it even without
authentication, so you can use that to get a "shareable link". You
can't "un-share" a photo that was shared in this way, though.

If you want to share a whole album (maybe you could even create an
album with just one photo in it), this functionality is built in to
PhotoPrism natively. You can use the share button and they have an
interface that lets you generate a shareable link, and revoke access
later, or set a time limit on how long the link will work.

If you want someone to access your whole library, because it is a
joint photo album, you can create them an account for PhotoPrism in
the settings page.

#### Editing photos

You can't edit photos in PhotoPrism directly. Use a separate
application. On Android I use Photo Editor by `dev.macgyver` and on
desktop I use <https://www.photopea.com/>, or Krita. You have to
download the photo, edit it in the external application, and then
upload it again as a new copy. Or, if you took the photo on your
phone, and it is still in your camera roll, you could edit it directly
there and upload the new version to PhotoPrism using PhotoSync.

#### Album and library sort order

One thing that's odd to me is there isn't a way to reorder the photos
in an album manually. You can set a sort order based on e.g. when the
photos were taken, but not tweak it manually. I have a thread [on the
bug tracker](https://github.com/photoprism/photoprism/issues/3867)
about this. The workaround at present is that if you want to change
the order of photos in an album, you can edit the photos and change
their "date taken" field, and then sort the album by that field.
That's what I have done for now but in the future I might try to
contribute a pull request to add a drag-and-drop feature.

Another minor issue: photos are sorted based on their naive date-time,
ignoring time zone. Yes, really. I guess this is a bug which will have
to be fixed eventually but the database query literally does an `ORDER
BY` on a column that has the non-time-zone aware version of the
date-time. References:
* <https://github.com/photoprism/photoprism/discussions/3780#discussioncomment-7351121>
  (point 3 and response from **@lastzero**)
* <https://matrix.to/#/!DqTvfyRmmWSkpOnbce:gitter.im/$EjU2dpiqZV5zuIS-CHule2boyfIOWxD7Vr7_UWPgHUk?via=gitter

Also, if EXIF data is insufficient to determine a timestamp, then
photos may get imported with timestamps based on their filename and in
this case the time zone may get set to `Local Time`. That is probably
incorrect because the local time on the server is UTC while the time
zone used in filenames is generally local to the phone (say, PT). The
best fix is probably to get PhotoSync to use UTC when automatically
naming photos, and then backfill appropriate timezones for "Local
Time" photos that were uploaded before fixing that (but after the
initial Google Photos import). I haven't bothered to do this because
it is at most a minor out-of-order issue.

### Full library import

Presumably you also have an existing photo library. There is support
for importing it, but because of the large amount of data, it's not
totally seamless to do quickly. There is official documentation from
PhotoPrism for [importing from Google
Photos](https://docs.photoprism.app/user-guide/use-cases/google/) and
[importing from Apple
Photos](https://docs.photoprism.app/user-guide/use-cases/apple/). In
this section I'll write about the former because that is what I had
personal experience with.

So, Google has an official data export tool, because it's required by
[GDPR](https://en.wikipedia.org/wiki/General_Data_Protection_Regulation)
and
[CCPA](https://en.wikipedia.org/wiki/California_Consumer_Privacy_Act).
You go to [Google Takeout](https://takeout.google.com/) and ask for a
download of your photos data. They email you a link.

If you don't have a large library you could maybe just download the
Takeout archive, unzip it, and upload the files to PhotoPrism from
home. I had a 30 GB photo library and I don't have gigabit ethernet at
home, so instead of doing it from home, where the download and upload
would take forever, I used the virtual server that I keep on hand to
run my backups and some other personal stuff. If you don't have one,
you could get something easily on a service like
[DigitalOcean](https://www.digitalocean.com/) or
[BuyVM](https://buyvm.net/) (the latter being what I use for this).
But you probably don't want to go that route unless you feel
comfortable with Linux and the command line.

For my usage, I found that the default settings for Takeout weren't
great for my library. 30 GB of photos with a max file size of 1 GB
meant 30+ zip files to download. So I re-did it with the maximum
possible file size so that everything would download in a single file,
and I changed to `.tar.gz` format instead of `.zip` because I felt
more comfortable with that on Linux. The way I got it to download to
my virtual server was by initiating the download from my laptop, then
cancelling it, copying the download URL from my browser, and
downloading that to my server using `wget` at the command line.

Assuming you're using PikaPods, to actually get the files into
PhotoPrism you have to use
[SFTP](https://en.wikipedia.org/wiki/SSH_File_Transfer_Protocol). You
can enable it in the PikaPods settings for your server and they give
you connection information. There are lots of graphical and
command-line SFTP clients but personally what I like the most is
[rclone](https://rclone.org/) because it does not just do SFTP but
also can interact with every other kind of cloud storage (e.g. Google
Drive, Nextcloud, Amazon S3, Backblaze) using the same interface. I
used `rclone config` to create a `webdav` remote with these settings,
taking the connection information from the PikaPods settings page:

```conf
[photoprism]
type = sftp
host = photos.intuitiveexplanations.com
user = p12345
pass = redacted
shell_type = unix
```

Then I could upload my photos library just by running:

```
rclone copy ./Takeout photoprism:import/ --progress --transfers=8
```

and waiting a few hours for everything to upload. I personally turned
off the Import feature in PhotoPrism settings during the upload, just
out of fear that having two things going at once would cause issues,
but I don't think that was necessary. The "import + index" process
took many more hours after the upload, once I turned it back on.

PhotoPrism [has
support](https://docs.photoprism.app/user-guide/use-cases/google/#metadata)
for reading the JSON files that Google Photos generates which have
photo metadata. This should work transparently for the most part. The
main thing to check is whether all the photos have valid timestamps.
In my case almost everything imported correctly, but there were a
small number of photos (~60) that had empty metadata (they showed up
when I searched for `Year` = `Unknown`). I investigated this and found
that this was definitely Google's fault - some of the photos just
didn't have JSON files next to them like they were supposed to. For
those ones I just fixed the metadata manually. (Sort of - I wrote a
little script to edit all the broken photos automatically, using the
PhotoPrism API.)

As documented in the [page on importing from Google
Photos](https://docs.photoprism.app/user-guide/use-cases/google/#transfer-albums),
albums aren't imported automatically. I used the [community
script](https://github.com/inthreedee/photoprism-transfer-album) they
linked with some success. One issue it had was that it didn't import
all the photos in the right *order* (see the comment above about album
ordering), and sometimes it imported duplicates in cases where I had
used the "edit" functionality in Google Photos. I think this bug is
Google's fault more than PhotoPrism. Anyway, I manually removed the
extra copies from my albums.

Some albums imported in the wrong order (see the section above on
album ordering), I just "fixed" these manually by tweaking the
timestamp metadata on the affected photos.
