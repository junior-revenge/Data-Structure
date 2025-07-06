# Problem 7.3 Jukebox
Design a musical jukebox using object-oriented principles.

## Questions for solving this problem.
1. This jukebox playing CD, MP3, Records? 
2. This jukebox need money?
3. It takes cash or card? 
4. How many users are there?

## Assumption
jukebox is a computer simulation that closely mirrors physical jukeboxes, and we'll assume that it's free. There is one user.  
The jukebox stores multiple CDs, but only one CD can be loaded into the CD player at a time. When a CD is not currently playing, it is stored back in the jukebox.

**Core Objects** : Jukebox, CD, Song, Playlists, Display  

**Actions** : CD selector, Playlist creation, Song selector, Queuing up a song, Get ne  xt song from playlist. 

### Implementation

```
import random
from collections import deque
from typing import Deque, Optional

class CD :
    def __init__(self, cd_id, artist, songs):
        self.cd_id = cd_id
        self.artist = artist
        self.songs = songs

class Song :
    def __init__(self, song_id, title, cd = None):
        self.cd = cd
        self.song_id = song_id
        self.title = title


class Playlist :
    def __init__(self):
        self.current_song = None
        self.queue = deque()

    def queue_up_song(self, song):
        self.queue.append(song)

    def play_next(self):
        pass

    def get_current_song(self):
        return self.current_song

    def shuffle(self):
        shuffled = list(self.queue)
        random.shuffle(shuffled)
        self.queue = deque(shuffled)

class CDPlayer :
    def __init__(self, cd:Optional[CD] = None,
                 playlist:Optional[Playlist]=None):
        self.cd = cd
        self.playlist= playlist
    # def play_song(self):
    #     pass
    def set_cd(self, cd):
        self.cd = cd

    def get_cd(self):
        return self.cd

    def set_playlist(self, playlist):
        self.playlist = playlist

class jukebox:
    def __init__(self, cd_player:CDPlayer, cd_collections:set,  
                 playlist:Playlist):
        self.cd_player = cd_player
        self.cd_collections = cd_collections
        self.playlist = playlist

    def get_current_song(self):
        return self.playlist.get_current_song

    def play_next_song(self):
        return self.playlist.play_next()

    def add_song_to_queue(self, song):
        self.playlist.queue_up_song(song)

    def shuffle_playlist(self):
        self.playlist.shuffle()

    def set_cd(self, cd):
        self.cd_player.set_cd(cd)

    def list_cd_collection(self):
        for cd in self.cd_collections:
            print(cd)


```


## Summary
1. Handle Ambiguity : When being asked an object-oriented design question, you should inquire who is going to use it and how they are going to use it. Depending on the question, you may even want to go through the "six Ws".  

2. Define the Core Objects : For example, suppose we are asked to do the object-oriented design for a restaurant. Our core objects might be things like Table, Guest, Party, Order, Meal, Employee, Server, and Host.  

3. Analyze Relationships : analyze the relationships between the objects. Which objects are members of which other objects? Do any objects inherit from any others? Are relationships many-to-many or one-to-many? - But we can often make incorrect assumptions. We should talk to our interviewer about how general purpose your design should be.  

4. Investigate Actions : We should have the basic outline of our OOD. What remains is to consider the key actions that the objects will take and how they relate to each other. We may find that we have forgotten some objects, and we will need to update our design.

