# MHF-Z-Custom-Quests
This is a repo for my MHFZ custom quests.

## How to use them
* Download both the quests and the questlists file.
* Import the questlists to your questlists in pgAdmin. (Read below on how to do that)
* Keep the questlists file inside the questlists folder inside your server bin folder.
* Extract the quests inside your quests folder, they have unique ID and will not overwrite any of the official quests.
* Just play and have fun!

## Custom Quests
**Road's Ancestral Challenger** is a musou-type quest that I made which you can face the Fatalis (Road's White Fatalis) outside the Hunter Road, the rewards are its materials!

**Freezing Hell** is another musou-type quest that I made to face Teo Tesukatora and Toa Tesukatora together, it's a quick and lethal quest though!

**Old Demon's Memories** is an HR100+ quest that I alongside my old friend Matahashi tried to make to recreate an old quest that was available in the Korean servers back them, I forgot to add the "Top Wing" as a reward, but this is the only way to get the worth Premium HR armours, but the quest is very tough and restricted!

## How to import questlists to pgAdmin by MAEL
1. Paste Malck quests into Erupe>Bin>Questlists
(All files the ones that begin like **list_** and **quest_**

2. Open pgAdmin4, navigate through databases>erupe>schemas>tables

3. Right click questlists table and select Scripts > (any scripts because you will paste mine, but if you are scared use INSERT)

4. Copy and paste this query and change parameters ill mark as **bold**

```
INSERT INTO public.questlists (ind, questlist) VALUES ('0', pg_read_binary_file('YourPathToErupe\Erupe\bin\questlists\quest_0_0.bin'));
INSERT INTO public.questlists (ind, questlist) VALUES ('42', pg_read_binary_file('YourPathToErupe\Erupe\bin\questlists\quest_42_2A.bin'));
INSERT INTO public.questlists (ind, questlist) VALUES ('84', pg_read_binary_file('YourPathToErupe\Erupe\bin\questlists\quest_84_54.bin'));
INSERT INTO public.questlists (ind, questlist) VALUES ('126', pg_read_binary_file('YourPathToErupe\Erupe\bin\questlists\quest_126_7E.bin'));
INSERT INTO public.questlists (ind, questlist) VALUES ('168', pg_read_binary_file('YourPathToErupe\Erupe\bin\questlists\quest_168_A8.bin'));
```

**path** is your full path to Erupe\Bin folder (copy it from windows exporer top bar. And yes you absolutely must use **\** and not **/**.

Example path will look like this:
C:\Erupe\bin\questlists\quest_0_0.bin

5. Click Run/Execute (little play button in top right) or press F5.

6. Press right on questlist>view data>all rows. Make sure you have 5 entries and second column has [binary data] type not [null]

## How to make your own questlists
Since I know that many of you want to have your own custom quests and even that beauty looking info in the quest counter, so I'm sharing my notes and hope it helps those who are interested in this. Just be warned that I couldn't spend much time here to find everything so there are a few missing things, if you happen to know and want to share it, you're free to share it as an issue or whatever. Be warned that this is basically a copy/paste of my notes, this is not exactly a step-by-step or whatever kind of tutorial, just sharing some info here. Also you can even add more quests to those files, I just didn't take the time to look for that so for now I don't know how to add more, but I assume that the limit is 42 per file, which means that the only one that is not "full" is the quest_168_A8.bin file. Also for some reason you cannot use more than 5 questlists files at the same time in the database, maybe it could be possible with a proper code?

**What do you need?**
Just a hex editor to do that.

All tests were made using the “quest_0_0.bin” which is a questlist for events.

Following the above file here’s what I got:
![image](https://user-images.githubusercontent.com/68492734/160055399-c3fae801-0c16-49a0-872b-95945adfe7d6.png)
Everything selected are the quest information, above it you can see that there are text and it belongs to the previous quest, below the selected zone there is the text for this quest. Keep that in mind.

Just at the start of the quest info there is this value which is related to the amount of players and the PVP modes.
04 = 4 players
![image](https://user-images.githubusercontent.com/68492734/160055683-c2287a7e-7b8a-47de-9c04-ad282618706e.png)

Just at the right of the players amount there is the value for the quest tab 1C 01 = G Event Tab.
27 01 = G Exotic Tab
12 01 = LR/HR Event Tab
![image](https://user-images.githubusercontent.com/68492734/160055723-06a2e373-d750-4f28-95d5-b249b5376abf.png)

This isolated value is the “mark” on the quest.
00 = None
01 = Recommended
02 = New
![image](https://user-images.githubusercontent.com/68492734/160252302-063e5a0b-1a68-4cca-a85f-aa8b0131947b.png)

Don’t touch this value, it seems to be unique for each quest, this disables all other quests.
![image](https://user-images.githubusercontent.com/68492734/160055749-e1baa38c-41ff-4638-a7e4-3022b0a7debb.png)

This is the value that changes the objectives 0A for Main/SubA/SubB or 4A for Master.
![image](https://user-images.githubusercontent.com/68492734/160055791-4944ff14-3029-4782-9883-9f55bb771ac0.png)

This is the value for the star lvl, it’s from 01~08. I thing that the 00 before it is also related to it…
![image](https://user-images.githubusercontent.com/68492734/160055836-a4dce7b5-373b-4733-a999-26332004b7d1.png)

This is the value for the quest fee, as usual those values are inverted “BC 02 00 00” hex 00 00 02 BC = dec 700.
![image](https://user-images.githubusercontent.com/68492734/160055869-51bfcfe0-f73b-455c-9ef0-1de164efa602.png)

Followed by the above quest fee there is the quest main reward value, as usual those values are inverted “A4 06 00 00” hex 00 00 06 A4 = dec 1700.
![image](https://user-images.githubusercontent.com/68492734/160055900-26f38a17-7ce6-48fc-8eee-778ee83ec0ca.png)

Finally found the value for the map icon, I’ll need to note one by one that I test… 0C is Dondruma.
![image](https://user-images.githubusercontent.com/68492734/160055929-e5ea93a7-5cce-479a-b24c-c38fba1ef6b2.png)

This is the special condition value, I suspect that the 00 before that belongs to that… I would need to search for other quests with conditions and note their values, probably inverted as always, this value is for “成功回数制限”, 00 for no restrictions. Changing the 00 before it result in crashing… don’t mess with it! Going above C3 will result in a crash!
No Restriction = 00 
![image](https://user-images.githubusercontent.com/68492734/160055958-2e4ff037-c6e4-4225-80f1-cd9d805d1676.png)

This is also related to the restrictions, these values are like a special restriction like required subscription.
Normal? = 02 12
Premium? = 03 13
![image](https://user-images.githubusercontent.com/68492734/160055992-09bd580e-7c9e-4d50-9de1-3f4ef58db44a.png)

This is the time, counted by frames hex 90 5F 01 = 90000 fps = 50min
![image](https://user-images.githubusercontent.com/68492734/160056002-f19af0ff-4f63-41d5-9c09-f8bd7dcd0929.png)

This is the first quest of the GR event list, Silver Rathalos.
This is the line and the quest ID “27 FC” inverted to hex FC 27 = dec 64551 which means a custom ID for a quest file, changing this to load that quest file. This is quite interesting, if I change it for quests that apparently are listed in one of the used files (questlists or mhfinf) it will also carries over all the configuration, but not at all for all cases.
![image](https://user-images.githubusercontent.com/68492734/160056029-afb04b94-d70a-4e00-a152-02af0a1dcbb5.png)

A few bytes to the right of the quest ID you can find this value which is the monster’s icon, need to check the monster’s ID to make the game look for that icon.
![image](https://user-images.githubusercontent.com/68492734/160056059-ca7b991a-227e-4efb-a0ab-c4b2202d5ce2.png)
This value is related to the monster Icon… Leave it as 01 to use the monster Icon…
![image](https://user-images.githubusercontent.com/68492734/160056091-53037a1c-6115-4d96-96b8-324076e30917.png)

This is the GR restriction, the first 2 bytes is the needed GR lvl to join, again this value is inverted “20 03” hex 03 20 = dec 800. The following 00 00 is the highest GR lvl to join, values inverted.
The following orange follows the same pattern for hosting player.
![image](https://user-images.githubusercontent.com/68492734/160056110-297465c5-e3c1-400d-bcfb-4dd17c0d65b0.png)

These values are for the equipment restrictions…
![image](https://user-images.githubusercontent.com/68492734/160056129-4c23c7c8-cbb6-42ed-bd16-a2f967301767.png)
Following the above, the first 2 bytes are for the “legs/boots”.
![image](https://user-images.githubusercontent.com/68492734/160056160-06a2b6e8-58af-4728-8413-decdefbacaca.png)
The next 6 bytes are for the 3 decos in the boots, remember that each deco is 2 bytes.
![image](https://user-images.githubusercontent.com/68492734/160056174-8c940326-1f54-4bb0-bc33-6399250078b5.png)
The weapon is the next, here I can change the rarity too, 0B 00 = Rare 11 weapon or below. I can use this value to force a specific weapon to be used, it seems that each value that is not a weapon’s ID turns to be a rarity number… I’m not feeling like I’ll try search anymore.
![image](https://user-images.githubusercontent.com/68492734/160056201-b5e96821-cff6-40a2-8822-e0e687c3a7bd.png)
The following 6 bytes are just the same as the other pieces, but it’s for the 3 weapons decos. Atcually messing with the first 2 bytes I can change the allowed weapons but there are a lot of values to test…
![image](https://user-images.githubusercontent.com/68492734/160056223-e85e79dc-556d-4cfd-a416-e5af304505fe.png)
After the weapon the next one is for the head piece.
![image](https://user-images.githubusercontent.com/68492734/160056242-a5aa27f0-8529-4685-b872-51f899ef8c89.png)
Then again the next 6 bytes for the decos.
![image](https://user-images.githubusercontent.com/68492734/160056256-c5bb7af0-db88-4f13-8985-f1011fbe3260.png)
Now for the chest piece.
![image](https://user-images.githubusercontent.com/68492734/160056275-fd020adb-64a5-4fd8-8646-365044af3af0.png)
Now the decos for the chest.
![image](https://user-images.githubusercontent.com/68492734/160056306-e29ca2f7-9d96-499e-8f07-2955e01cff9f.png)
Values for the arm.
![image](https://user-images.githubusercontent.com/68492734/160056328-47ebf3eb-44e8-4c6e-aae4-f31d7709e6c7.png)
The decos for the arm.
![image](https://user-images.githubusercontent.com/68492734/160056350-cd9acc25-8ce9-48e5-9c88-c8e7ff527c17.png)
Values for the waist piece.
![image](https://user-images.githubusercontent.com/68492734/160056368-b1dc73a9-8ce3-4637-b5b5-5c196a9d1bb6.png)
And the decos.
![image](https://user-images.githubusercontent.com/68492734/160056392-84abc436-f315-419c-8bf8-7b3239a41318.png)

This single value is the “mode” 0C = Unlimited (UL), if you add this the quest will become GR.
04 = Hardcore (HC)
06 = Forced HC
0B = Forced GHC
![image](https://user-images.githubusercontent.com/68492734/160252356-41ce947c-00ae-46bc-a6da-98f9ba7798f4.png)

This is the main GRP value showed ingame, it’s also inverted “84 03 00 00” to hex 00 00 03 84 = dec 900. Somehow it supports a HUGE amount of points… The following values in orange follows the same pattern as they are for SubA and SubB.
![image](https://user-images.githubusercontent.com/68492734/160056417-4d48cb64-f1f1-4caa-82f2-93c988d53ac2.png)

After the GRP values we have the reward mats showed ingame. Each material is 2 bytes, so “A9 3D” is one mat and it’s inverted so hex 3D A9 = dec 15785 (霧爆凝華) and the others follow the same pattern, max of 3 mats per quest is showed.
![image](https://user-images.githubusercontent.com/68492734/160056444-9302f7ce-e5cb-4a84-b7c6-bd30731be1f7.png)

These values seems to be the pointers… for the text at least… I need to work more on these.
![image](https://user-images.githubusercontent.com/68492734/160056469-651dcbff-8a5f-4c64-b864-d2cf1aa71b2d.png)
Related to the pointers above I found something interesting, those values are for the text below, to use it properly there is only the start pointer, in this case the value 99 refers to the orange 00 value a few lines below it, add a +1 hex value will move it to right and then below, the original value was A5 which points to the start of the black space I left (the 00 between the 20 values in orange). A bit confusing but that’s what I found. Use the below for comparison.
![image](https://user-images.githubusercontent.com/68492734/160056503-64055569-1e0d-404d-b4b9-08efdddb92b9.png)

## Note: The questlists are my rebalanced version, check my personal adjustments repo to know more about it.
