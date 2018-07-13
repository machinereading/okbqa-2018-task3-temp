# [OKBQA7 2018 Task3](http://7.okbqa.org/task/3)

## Multimodal Character Identification on Videos

This taks is to link each mention as a certain character in dialogue, given dialouge text and corresponding video. 
Let a mention be a nominal referring to a person (e.g., *she*, *mom*, *Judy*), and an entity be a character in a dialogue. 

![Example](https://image.ibb.co/fm4iP8/multi_modal_character_identification.png)

## References

* SemEval 2018 

## Task Organizers

* Key-Sun Choi, SWRC, KAIST.
* Kijong Han, SWRC, KAIST.
* Giyeon Shin, SWRC, KAIST

## Datasets

The first two seasons of the TV show Friends are annotated for this task. 
Each season consists of episodes, each episode comprises scenes, and each scene is segmented into sentences. 
The followings describe the distributed datasets:

* [friends.train.episode_delim.conll](dat/friends.train.episode_delim.conll): the training data where each episode is considered a document.
* [friends.train.scene_delim.conll](dat/friends.train.scene_delim.conll): the training data where each scene is considered a document.
* [friends.test.episode_delim.conll](dat/friends.test.episode_delim.conll): the test data where each episode is considered a document.
* [friends.test.scene_delim.conll](dat/friends.test.scene_delim.conll): the test data where each scene is considered a document.
* [friends.test.episode_delim.conll.nokey](dat/friends.test.episode_delim.conll.nokey): same as [friends.test.episode_delim.conll](dat/friends.test.episode_delim.conll); the gold keys are replaced by `-1`.
* [friends.test.scene_delim.conll.nokey](dat/friends.test.scene_delim.conll.nokey): same as [friends.test.scene_delim.conll](dat/friends.test.scene_delim.conll); the gold keys are replaced by `-1`.

Note that the evaluation sets did not include the gold keys during the competition; we made them available after the competition.
No dedicated development set was distributed for this task; feel free to make your own development set for training or perform cross-validation on the training sets.

## Format

All datasets follow the CoNLL 2012 Shared Task data format.
Documents are delimited by the comments in the following format:

```
#begin document (<Document ID>)[; part ###]
...
#end document
```

Each sentence is delimited by a new line ("\n") and each column indicates the following:

1. Document ID: `/<name of the show>-<season ID><episode ID>` (e.g., `/friends-s01e01`).
1. Scene ID: the ID of the scene within the episode.
1. Token ID: the ID of the token within the sentence.
1. Word form: the tokenized word.
1. Part-of-speech tag: the part-of-speech tag of the word (auto generated).
1. Constituency tag: the Penn Treebank style constituency tag (auto generated).
1. Lemma: the lemma of the word (auto generated).
1. Frameset ID: not provided (always `_`).
1. Word sense: not provided (always `_`).
1. Speaker: the speaker of this sentence.
1. Named entity tag: the named entity tag of the word (auto generated).
1. Start Time : start time of the utterance on video.
1. End Time : end time of the utterance on video.
1. Entity ID: the entity ID of the mention, that is consistent across all documents.

Here is a sample from the training dataset:

```
/friends-s01e01  0  0  He     PRP   (TOP(S(NP*)    he     -  -  Monica_Geller   *  02:30 ~ 02:32 (284)
/friends-s01e01  0  1  's     VBZ          (VP*    be     -  -  Monica_Geller   *  02:30 ~ 02:32 -
/friends-s01e01  0  2  just   RB        (ADVP*)    just   -  -  Monica_Geller   *  02:30 ~ 02:32 -
/friends-s01e01  0  3  some   DT        (NP(NP*    some   -  -  Monica_Geller   *  02:30 ~ 02:32 -
/friends-s01e01  0  4  guy    NN             *)    guy    -  -  Monica_Geller   *  02:30 ~ 02:32 (284)
/friends-s01e01  0  5  I      PRP  (SBAR(S(NP*)    I      -  -  Monica_Geller   *  02:30 ~ 02:32 (248)
/friends-s01e01  0  6  work   VBP          (VP*    work   -  -  Monica_Geller   *  02:30 ~ 02:32 -
/friends-s01e01  0  7  with   IN     (PP*))))))    with   -  -  Monica_Geller   *  02:30 ~ 02:32 -
/friends-s01e01  0  8  !      .             *))    !      -  -  Monica_Geller   *  02:30 ~ 02:32 -
```
```
/friends-s01e01  0  0  C'mon  VB   (TOP(S(S(VP*))  c'mon  -  -  Joey_Tribbiani  *  -
/friends-s01e01  0  1  ,      ,                 *  ,      -  -  Joey_Tribbiani  *  -
/friends-s01e01  0  2  you    PRP           (NP*)  you    -  -  Joey_Tribbiani  *  (248)
/friends-s01e01  0  3  're    VBP            (VP*  be     -  -  Joey_Tribbiani  *  -
/friends-s01e01  0  4  going  VBG            (VP*  go     -  -  Joey_Tribbiani  *  -
/friends-s01e01  0  5  out    RP           (PRT*)  out    -  -  Joey_Tribbiani  *  -
/friends-s01e01  0  6  with   IN             (PP*  with   -  -  Joey_Tribbiani  *  -
/friends-s01e01  0  7  the    DT             (NP*  the    -  -  Joey_Tribbiani  *  -
/friends-s01e01  0  8  guy    NN            *))))  guy    -  -  Joey_Tribbiani  *  (284)
/friends-s01e01  0  9  !      .               *))  !      -  -  Joey_Tribbiani  *  -
```

A mention may include more than one word:

```
/friends-s01e02  0  0  Ugly         JJ   (TOP(S(NP(ADJP*  ugly         -  -  Chandler_Bing  *  (380
/friends-s01e02  0  1  Naked        JJ                *)  naked        -  -  Chandler_Bing  *  -
/friends-s01e02  0  2  Guy          NNP               *)  Guy          -  -  Chandler_Bing  *  380)
/friends-s01e02  0  3  got          VBD             (VP*  get          -  -  Chandler_Bing  *  -
/friends-s01e02  0  4  a            DT              (NP*  a            -  -  Chandler_Bing  *  -
/friends-s01e02  0  5  Thighmaster  NN               *))  thighmaster  -  -  Chandler_Bing  *  -
/friends-s01e02  0  6  !            .                *))  !            -  -  Chandler_Bing  *  -
```

The mapping between the entity ID and the actual character can be found in [`friends_entity_map.txt`](dat/friends_entity_map.txt).
