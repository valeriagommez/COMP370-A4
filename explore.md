**Indicate the  commands used to obtain the results.**

- **How big is the dataset?**
  - We can obtain this by doing `tail -n +2 data/clean_dialog.csv | wc -l`. It will give us the number of data rows (not counting the header row) : 36859

- **What’s the structure of the data? (i.e., what are the field and what are values in them)**
  - We can get a snapshot of the data by printing out the first two lines of the csv file : `head -n 2 data/clean_dialog.csv`
- This gives us the header row as well as an example of a data row with its values : 
```
"title","writer","pony","dialog"
"Friendship is Magic, part 1","Lauren Faust","Narrator","Once upon a time, in the magical land of Equestria, there were two regal sisters who ruled together and created harmony for all the land. To do this, the eldest used her unicorn powers to raise the sun at dawn; the younger brought out the moon to begin the night. Thus, the two sisters maintained balance for their kingdom and their subjects, all the different types of ponies. But as time went on, the younger sister became resentful. The ponies relished and played in the day her elder sister brought forth, but shunned and slept through her beautiful night. One fateful day, the younger unicorn refused to lower the moon to make way for the dawn. The elder sister tried to reason with her, but the bitterness in the young one's heart had transformed her into a wicked mare of darkness: Nightmare Moon."
```

- **How many episodes does it cover?**
  - We can use the title of each different episode by exploring the `"title"` field in the csv file. For this, we need the `csvtool` command and a few pipelines : `csvtool col 1 | data/clean_dialog.csv | tail -n +2 | sort | uniq | wc -l`
    - `tail -n +2` drops the first row (header)
    - `sort | uniq` drops repeated episode names
    - `wc -l` gets the number of unique episode names

- **During the exploration phase, find at least one aspect of the dataset that is unexpected – meaning that it seems like it could create issues for later analysis.**
  - By looking at the data, I noticed that line 34994 has a weird value for the "pony" field : `"My happy village lay in ruins Relationships got worse Spoiler alert"`. This name definitely doesn't correspond to a pony, and thus could skew whatever analysis we're trying to do.
    - The full row is : `"Sounds of Silence","Gregory Bonsignore","My happy village lay in ruins Relationships got worse Spoiler alert","we quickly learned That words could be a curse"`
  - Along the same note, I noticed that the field `"pony"` was often populated by `"Others"`, which can pose a problem. If our goal would be to study the frequency at which each pony talks during the show, having extra characters be categorized under `"Others"` would drastically skew our results, as that 'character' has 980 lines of dialogue (found using `grep "Others" data/clean_dialog.csv | wc -l`).


