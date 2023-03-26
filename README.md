<h1 align="center">
Duplicates Identification with custom Kmeans implementation
</h1>

<p align="center">This project aims to find duplicates from the Music_Brainz dataset using techiniches such as phonetic blocking, Levenshtein distance and a custom Kmeans implementation. It was a group work developed in the Database Project class from computer science course. UFAPE 2022.1 </p>

Members:
- ALLYSSON GUIMARÃES DOS SANTOS SILVA
- HERMYSON CASSIANO DE MEDEIROS OLIVEIRA
- JOANNE GABRIELA DOS SANTOS SILVA
- MANOEL GUSTAVO GALINDO DE SALES

## 📜 About
The Music_Brainz dataset can be found [here](https://dbs.uni-leipzig.de/research/projects/object_matching/benchmark_datasets_for_entity_resolution).
It has 20k, 200k and 2M datasets, for this project we used the 20k and 200k only. The data looks like this:
```
TID: a unique record's id (in the complete dataset).
CID: cluster id (records having the same CID are duplicate)
CTID: a unique id within a cluster (if two records belong to the same cluster they will have the same CID but different CTIDs). These ids (CTID) start with 1 and grow until cluster size.
SourceID: identifies to which source a record belongs (there are five sources). The sources are deduplicated.
Id: the original id from the source. Each source has its own Id-Format. Uniqueness is not guaranteed!! (can be ignored).
number: track or song number in the album.
title: title of the track.
album: album of the track.
length: the length of the track.
artist: the interpreter (artist or band) of the track.
year: date of publication.
language: language of the track.
```

First, we divided the dataset into **blocks** sorted by the phonetic from the music titles. This way musics with titles that sound closily are together. Then we ran our custom **kmeans algorithm** to **clusterize** each block with the **duplicates** found, it is a much more expensive step since it does a lot of comparisons, this is why we divided the base into blocks first.

For our kmeans implementation, we created a **custom distance function** that would calculate the distance between a given element and an array of rows (elements to compare). The string distances (title, album and artists) were calculated using Levenstein, years and track_numbers distances were calculated by outputing a distance of 0 if equal, and 1 (max distance) otherwise. These attributes distances were computed and the weighted average distance from the given element to all the rows passed is returned.

The **Kmeans** implementation works by selecting the 1st item as a **centroid**. Then we loop over all the items and compute the distance of the current item to all the centroids (we start with only one), using our custom distance function, passing the current element and the array of centroids to compare, we get the lowest computed distance, if that distance is lower then a given **threshold** (we default to 0.4) then we consider the current item a **duplicate** to that centroid and add this item to that centroid cluster, otherwise, that element was not considered a duplicate to any of the centroids, so we set that item as a centroid as well, and the iteration continues.
