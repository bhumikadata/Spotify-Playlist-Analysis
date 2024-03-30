# Spotify-Playlist-Analysis

# Problem Statement

Spotify, as a leading music streaming service, strives to understand user preferences and engagement patterns to enhance user experience. One aspect of user engagement is playlist interaction – users create playlists, follow others' playlists, and listen to tracks within these playlists. Identifying the most popular tracks across various playlists can provide valuable insights for content curation, recommendation systems, and promotional activities.

![DAy25](https://github.com/bhumikadata/Spotify-Playlist-Analysis/assets/131578649/1ec75f1f-e556-4f37-b7c3-935fed028b7a)

# SQL Code

```
WITH playlist_engagement AS (
    SELECT
        p.playlist_id,
        t.track_id
    FROM
        playlist_plays p
    JOIN
        playlist_tracks t ON p.playlist_id = t.playlist_id
    GROUP BY
        p.playlist_id,
        t.track_id
    HAVING
        COUNT(DISTINCT p.user_id) >= 2
),
track_popularity AS (
    SELECT
        track_id,
        COUNT(DISTINCT playlist_id) AS no_of_playlist,
        ROW_NUMBER() OVER (ORDER BY COUNT(DISTINCT playlist_id) DESC) AS rn
    FROM
        playlist_engagement
    GROUP BY
        track_id
)
SELECT
    track_id,
    no_of_playlist
FROM
    track_popularity
WHERE
    rn <= 2;
```

# Explanation
The SQL query consists of two Common Table Expressions (CTEs):

### playlist_engagement: 
Filters playlists that were played by at least two distinct users.

### track_popularity: 
Calculates the number of playlists each track is part of and assigns a row number based on popularity.

The final SELECT statement retrieves the top 2 tracks based on their popularity.

# Conclusion 

This SQL query can be utilized by Spotify's data analysts to gain insights into the most popular tracks among users. By identifying tracks that appear in multiple playlists played by different users, Spotify can prioritize these tracks for promotions, recommendations, or algorithmic playlists, ultimately enhancing user engagement and satisfaction.
