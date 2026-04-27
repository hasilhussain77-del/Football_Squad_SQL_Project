
 FOOTBALL SQUAD SELECTION (SQL PROJECT)



 1. PLAYER DISTRIBUTION BY POSITION
 Shows number of players in each position
SELECT position, COUNT(*) AS count_of_players
FROM filtered_players
GROUP BY position
ORDER BY count_of_players DESC;


 2. PLAYER DISTRIBUTION BY NATION
 Shows how players are distributed across countries
SELECT nations, COUNT(*) AS count_of_players
FROM filtered_players
GROUP BY nations
ORDER BY count_of_players DESC;


 3. PERFORMANCE ANALYSIS BY POSITION
 Aggregates goals, assists, and saves by position
SELECT 
    position,
    SUM(goals) AS total_goals,
    SUM(assists) AS total_assists,
    COALESCE(SUM(saves), 0) AS total_saves
FROM filtered_players
GROUP BY position
ORDER BY total_goals DESC, total_assists DESC, total_saves DESC;


 4. TOP PERFORMERS IDENTIFICATION
 Combines top goal scorer, assist provider, and goalkeeper in one table
SELECT * FROM (
    SELECT 'Top Goal Scorer' AS category, player, goals AS value
    FROM filtered_players
    ORDER BY goals DESC
    LIMIT 1
)

UNION ALL

SELECT * FROM (
    SELECT 'Top Assist Provider' AS category, player, assists AS value
    FROM filtered_players
    ORDER BY assists DESC
    LIMIT 1
)

UNION ALL

SELECT * FROM (
    SELECT 'Top Saves (GK)' AS category, player, saves AS value
    FROM filtered_players
    WHERE position = 'GK'
    ORDER BY saves DESC
    LIMIT 1
);


 5. CREATE SCORING MODEL
Assigns a performance score based on position-specific metrics
CREATE TABLE player_scores AS
SELECT 
    player_id,
    player,
    position,
    nations,
    min AS experience,

    CASE 
        WHEN position = 'FW' 
        THEN (goals * 5 + assists * 3 - fouls * 2)

        WHEN position = 'MF' 
        THEN (goals * 3 + assists * 4 - fouls * 0.25)

        WHEN position = 'DF' 
        THEN (assists * 2 - red_cards * 1)

        WHEN position = 'GK' 
        THEN (saves * 5 - fouls * 2)
    END AS score

FROM filtered_players;


 6. FINAL TEAM SELECTION (4-3-3 + SUBSTITUTES)
 Ranks players within each position and selects top players
SELECT 
    player,
    position,
    score,

    CASE 
        -- Starting XI
        WHEN position = 'GK' AND rnk = 1 THEN 'Starting XI'
        WHEN position = 'DF' AND rnk <= 4 THEN 'Starting XI'
        WHEN position = 'MF' AND rnk <= 3 THEN 'Starting XI'
        WHEN position = 'FW' AND rnk <= 3 THEN 'Starting XI'

        -- Substitutes
        WHEN position = 'GK' AND rnk = 2 THEN 'Substitute'
        WHEN position = 'DF' AND rnk BETWEEN 5 AND 6 THEN 'Substitute'
        WHEN position = 'MF' AND rnk = 4 THEN 'Substitute'
        WHEN position = 'FW' AND rnk BETWEEN 4 AND 5 THEN 'Substitute'
    END AS squad_role

FROM (
    SELECT *,
           ROW_NUMBER() OVER (PARTITION BY position ORDER BY score DESC) AS rnk
    FROM player_scores
) t

WHERE 
(position = 'GK' AND rnk <= 2)
OR
(position = 'DF' AND rnk <= 6)
OR
(position = 'MF' AND rnk <= 4)
OR
(position = 'FW' AND rnk <= 5);


 END OF PROJECT
This script performs:
✔ EDA
✔ Performance Analysis
 ✔ Scoring Model
 ✔ Final Squad Selection (4-3-3)
