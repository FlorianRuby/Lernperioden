# Project idea
A customisable profile card website showcasing one's LastFM activity and various links. 

### Frontend:
↳ Landing <br>
↳ Profile <br>
↳ Dashboard / Settings

I'm using React (jsx) in combination plain CSS for the frontend.

**Profile Showcase** <br>
![sneakpeek2-ezgif com-optimize](https://github.com/user-attachments/assets/8aabc094-4021-454b-b484-ab2ab5306264) <br>
 <br><br>
**Settings Showcase** <br>
Customise <br>
<img src="https://github.com/user-attachments/assets/ad1cf7a0-53ac-4ee0-84bf-49def084d565" width="1000" /> <br>
<img src="https://github.com/user-attachments/assets/314e9215-b9c6-48aa-9868-fc3ca77a1e88" width="1000" /> <br>

Links <br>
<img src="https://github.com/user-attachments/assets/a93405b9-bdd6-49f7-ae5b-ced07682a2db" width="1000" /> <br>

Cards <br>
<img src="https://github.com/user-attachments/assets/ae7ff918-0b1a-470f-91db-f113950db0f3" width="1000" /> <br>



In the settings you can customise pretty much everything:
1. colors for virtually everything
2. profile / background picture
3. bio text
4. effects for the text (typewriter, glow)
5. various opacities
6. links
7. Font type
8. Font color (for headers, text, subtext)
9. custom cursors
10. cursor trails
11. custom audios
12. custom boxes (embed of link with extra information such as Github followers or Youtube followers, amount videos & description)
13. ...

### Backend

The page is mainly static and doesn't require as server, yet it still needs a backend for the API. Currently I'm retrieving information about the user's Discord activity through a Discord bot written in JavaScript using Discord.JS. 
The website is currently fetching the LastFM data each time the users profile is loaded which is not good rate limt wise (using the Spotify API for covers which is even more limited request wise). That's why when I want to launch the website I need to fetch each users LastFM data and store it in the Database. For this I want to use Rust as I'm currently learning it and I think I'm now on a level where I can try to use it in this project. If I succeed in that then I'll rewrite the Discord bot in Rust so everything uses the same programming language and as overal training. 

**Database showcase** <br>
For the Database I'm using Supabase as it's easy to set up and it has CDN which makes everything much faster when using the website from around the world in comparision to hosting the database locally on my Raspberry-Pi. 

<img src="https://github.com/user-attachments/assets/ce9e99ce-8dda-41db-89f1-b91df0ebdfd1">

On the left side are the tables that contain text, links, images (buckets) etc. 
On the right side are tables that store data about the styling, for example color, font, opacity etc.

## Make or break features
1. Profile page MUST load fast
2. Settings should be clear and the user should have a good experience while creating his profile
3. LastFM features work all the time (no missing covers => API limits)

## Check progress
> Check issues & milestones [here](https://github.com/Dajialol/dajia.lol)

Eventually I'll move the backend (Discord bot & API fetching) into its own repository but currently everything is in this one repository)


---

## April 25

Save users LastFM data in DB (instead of fetching everytime when the users profile is opened = causes Spotify API ratelimits)
- [X] write documentation about current progress, explain frontend, backend, DB etc
- [X] create DB tables for the LastFM Data 
- [X] look at how to use APIs in Rust (reqwest, tokio etc) - Don't think I can fully implement it in 45min.

✍️Today I wrote the documentation about where I currently am regarding my project as well as a detailed explanation of the project itself and how I'm implementing it. I also created the tables that I'll need to store the API data later on. I added the current playing song to the profile_lastfm as well as a top artist, top track and top album table. For each of those tables I have a column for each section of the API response that LastFM provides along with a column for the time range. I also looked inot how to implement an API in Rust, which I didn't really know how to do before, I just knew I had to use tokio, serde and reqwest. I also added the Supabase connection so when the Rust project is run the current playing song is saved in the profile_lastfm table. 

The code for the Rust backend is [here](https://github.com/Dajialol/lastfm). <br><br>

<br>

## May 2
- [x] Save 'Top ...' in DB (every hour)
- [X] Save 55 most recent songs in extra table (not created yet)
- [X] Update 'Top ... ' if current playing song is somewhere in there (could use hashmap => figure out if that's possible to do while the data is in the DB => how)

Deviceless
- [X] Look at DB structure, what is unclear, is it worth cleaning up? 

✍ Today I was able to implement the Rust backend part that fetches the most recent tracks as well as the top tracks / artists and albums from a user. I've added a snippet from the DB below. With that being said, I wasn't able to host it on my RPI yet as I've yet to install Rust on there.

I've also taken a look at the DB structure and while it's a bit confusing and not perfectly organised it works and I don't think it's worth the time to change something that works, especially since no one besides me has to understand the DB.


Example: top track

| timeframe | rank | track_name | artist_name | playcount | mbid | url | image_url | fetched_at |
|-----------|------|------------|-------------|-----------|------|-----|-----------|-------------|
| 7day      | 1    | Sky        | d4vd        | 30        |      | link | link | time in utc |
| 1month    | 5    | DTN        | d4vd        | 85        |      | link | link | time in utc |

<br>

timeframe: either 7day, 1month, 12month, overall <br>
mbid: musicbrainz ID, gets fetched automatically, however LastFM doesn't always provide one. <br>
url: LastFM page URL <br>
image_url: Cover image is sourced from LastFM or Spotify, depending on availability. Spotify covers are preferred for their quality, but they’re not always accessible, especially for newly released albums. I've also had this issue with other albums but haven't found the reason for that yet. LastFM works as a fallback fairly well however there are still sometimes albums/ artist pictures that use a placeholder. <br>
fetched_at: time it was fetched at. Don't really have a practical reason for it but it's nice for debugging. <br>

## May 9
Host website
- [X] server, pm2 & cloudflared tunnel config
- [X] host backend (install Rust)
- [X] test website & create issues for bugs

✍️Today I was able to set up the website on my Raspberry Pi and it's now online under [dajia.lol](http://dajia.lol). I first had a lot of struggles with the NGINX reverse proxy config, that's why I tried Caddy which worked perfectly from the start. I was also able to work on these GH issues: #36 & #37

## May 16
work on GH issues
- [x] #34
- [x] #52
- [x] #47
- [x] #43

 I worked on some GH issues, over the past week I was able to clear my backlog from about 20 open issues to only 7 of which most are features and not bugs. I specifically finished these issues: #44 #52 #47 #43 . For the remainder of the day I tackled #46 - templates . Which in my opinion is a make or break feature since with templates you don't have to set up everything yourself which could take a long time especially since my settings UI/UX isnt that intuitive. This is also good since I can just link to the background image and other settings and don't have to upload another image for every user. I'm not done with this feature yet and will hopefully finish implementing it next week. 

## May 23
- [X] templates category is showing up in settings
- [X] add options for templates (backend & frontend)
- [X] templates work in user profile and are dispalys correctly
- [ ] add public documentation for templates / guide user in settings as well

✍️Today I worked on the templates feature and was able to inplement it. I had some troubles as my database table structure is a bit messy and I have different things I need to save for the template in different tables. However I'm not gonna change my db structure as it works and that's just unecessary work. The templates function now however I was only able to polish the frontend for general templates page yet but wasn't able to create a nice frontend for the template creation yet.
Currently I have one question and worry, in the templates settings page you can see a preview for every template with the actual user info displayed. So there can be a lot of backgrounds/avatars loaded from the DB in a short period of time and I'm unsure if a bigger userbase than just a few friends and me would destroy my EGRESS limits. I might have to localise my DB which would be very annoying and I'd lose access to CDN as well and I'd have to worry about SQL injection and other decurity concerns as wel as supabase currebtly handles those.

## June 6
- [X] polish create template UI
- [ ] add public documentation for templates - guide user in settings
- [X] test out EGRESS usage and find a solution if it's too much.
- [X] #56 - Can't change typewriter speed

possible egress solution? comprimise images for preview? not sure if that eould work since we essentially use the public.profile and just use different values, I really don't want to change and risk anything with the public.profile

✍️This week I finished the template UI, there are still things I want to add to make the UX better such as being able to favorite templates, being able to search for tags/names, a section where you can quickly find the templates you created and the one's you've favorited etc. However I was able to finish the base version and I'm testing it now. I was also able to make the entire website responsive for mobile, including the settings page. I looked at the Egress and it didn't look too bad, however if dajia attracts more users I'm gonna have to host supabase locally. (supabase is opensource so you can start using it online for free and when I go over the usage limits I can simply host it locally)
Yesterday I was also able to 'launch' Dajia and move on from the testing phase since I feel like it's polished enough and I couldn't find any major site breaking bugs, there are still a few which I'll be fixing within the next week.

Another solution to the egress problem is setting up an actual homelab with my old pc which I'm also [planning](https://github.com/reazndev/server).
However I can't implement that till Tuesday next week so I'll just have to pray that my egress usage isn't exploding till then.

## June 13
- [X] #57 - Particles stack on site when site minimised and open in the background -> limit amount of particles on the screen at once & start deleting old ones
- [ ] #55 - Custom cursor not working - find out why
- [ ] #55 - Custom cursor not working - implement fix

✍️Today I was only able to successfully fix the partiles stackin too much by limiting the amount of particles that can be on the site at once to 35. When the cap is reached and a new particles is generated the oldest one gets deleted. I tried out a bunch of different cap values and I'm pretty happy with the limit of 35 performance but also aesthetic wise. I sadly wasn't able to figure out the issue with the custom cursors.

# June 20
- [X] clean up code
- [X] take it easy (already done plenty and I'm out of things to do)

✍️Today I've decided to take it easy and just clean up the code, read through the code again and undestand it since I've been working on this project for more than 3 months now. I also thought about ways of presenting Dajia next week which isn't as easy as I initially thought since the main point of Dajia is the LastFM integration and I doubt that more than 5 people know about LastFM in my class. Also another huge point is customisation and that's obviously rather hard to show since I can't let everyone create a new profile. I thought that I could make a small list with a few different profiles and then one could see the possibilities via that and also create a showcase profile where my classmates can change things without harming my personal account. Also that'd be a cool relique.

In the end I'm very happy with Dajia and how it turned out, however I've also invested way more time than the time we've been given in the Lernatelier. I'm tracking my time via Wakatime and I've come to the conclusion that I've probably spent 120-150 hours in total and that doesn't even include server work which I've also wasted a lot of time on. Also I of course utilised AI to a certain degree since there's no way I can figure out a lot of the stuff in the time that I've been given (for example for my hosting setup I've used a lot of AI because fuck NGINX and other things like CORS and Regex that I just couldn't be bothered to invest time into)
