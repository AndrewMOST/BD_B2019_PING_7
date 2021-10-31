# ДЗ 6

### Задача 1

```sql
SELECT EXTRACT(YEAR FROM p.birthdate), COUNT(DISTINCT p.player_id), COUNT(res.medal)
FROM Players p
    JOIN results res ON p.player_id = res.player_id
    JOIN events ev ON res.event_id = ev.event_id
    JOIN olympics oly ON ev.olympic_id = oly.olympic_id
WHERE res.medal = 'GOLD' AND oly.year = 2004
GROUP BY EXTRACT(YEAR FROM p.birthdate);
```

### Задача 2

```sql
SELECT e.event_id FROM Events e
JOIN Results r ON e.event_id = r.event_id
WHERE is_team_event = 0 AND r.medal = 'GOLD'
GROUP BY e.event_id HAVING count(r.medal) >= 2;
```

### Задача 3

```sql
SELECT DISTINCT p.name, e.olympic_id
FROM Players p
    JOIN Results r ON p.player_id = r.player_id
    JOIN Events e ON e.event_id = r.event_id
WHERE r.medal IN ('GOLD', 'SILVER', 'BRONZE');
```

### Задача 4

```sql
SELECT vowels.country_id
FROM (
    SELECT Players.country_id, COUNT(*) AS vowel_num
    FROM Players
    WHERE UPPER(LEFT(Players.name, 1)) IN ('A', 'E', 'I', 'O', 'U')
    GROUP BY Players.country_id
    ) AS vowels
JOIN (
    SELECT Players.country_id, count(*) AS all_num
    FROM Players GROUP BY Players.country_id
    ) AS players ON vowels.country_id = players.country_id
ORDER BY CAST(vowel_num AS decimal) / players.all_num DESC
LIMIT 1
```

### Задача 5

```sql
SELECT Countries.country_id
FROM Olympics
    JOIN Events ON Events.olympic_id = Olympics.olympic_id
    JOIN Results ON Events.event_id = Results.event_id
    JOIN Players ON Players.player_id = Results.player_id
    JOIN Countries ON Countries.country_id = Players.country_id
WHERE Olympics.year = 2000 AND Events.is_team_event = 1
GROUP BY Countries.country_id, Countries.population
ORDER BY CAST(COUNT(Results.medal) AS DECIMAL) / Countries.population
LIMIT 5;
```
