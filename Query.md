Documentação Query - BigQuery

Melissa de Oliveira Pecoraro - Jornada de Dados - Projeto 02

🔍QUERY:

5.1.2 🔵 Identificar e tratar valores nulos




```SQL
SELECT
  COUNT(*)
FROM
  `laboratoria-projeto2-457801.projeto2.1`
WHERE track_name IS NULL
```
```SQL
SELECT
  SUM(CASE WHEN track_id IS NULL THEN 1 ELSE 0 END) AS track_id_nulls,
  SUM(CASE WHEN track_name IS NULL THEN 1 ELSE 0 END) AS track_name_nulls,
  SUM(CASE WHEN artists_name IS NULL THEN 1 ELSE 0 END) AS artists_name_nulls,
  SUM(CASE WHEN artist_count IS NULL THEN 1 ELSE 0 END) AS artist_count_nulls,
  SUM(CASE WHEN released_year IS NULL THEN 1 ELSE 0 END) AS released_year_nulls,
  SUM(CASE WHEN released_month IS NULL THEN 1 ELSE 0 END) AS released_month_nulls,
  SUM(CASE WHEN released_day IS NULL THEN 1 ELSE 0 END) AS released_day_nulls,
  SUM(CASE WHEN in_spotify_playlists IS NULL THEN 1 ELSE 0 END) AS in_spotify_playlists_nulls,
  SUM(CASE WHEN in_spotify_charts IS NULL THEN 1 ELSE 0 END) AS in_spotify_charts_nulls,
  SUM(CASE WHEN streams IS NULL THEN 1 ELSE 0 END) AS streams_nulls,

FROM
  `laboratoria-projeto2-457801.projeto2.1`
```
```SQL
SELECT
  SUM(CASE WHEN track_id IS NULL THEN 1 ELSE 0 END) AS track_id_nulls,
  SUM(CASE WHEN in_apple_playlists IS NULL THEN 1 ELSE 0 END) AS in_apple_playlists_nulls,
  SUM(CASE WHEN in_apple_charts IS NULL THEN 1 ELSE 0 END) AS in_apple_charts_nulls,
  SUM(CASE WHEN in_deezer_playlists IS NULL THEN 1 ELSE 0 END) AS in_deezer_playlists_nulls,
  SUM(CASE WHEN in_deezer_charts IS NULL THEN 1 ELSE 0 END) AS in_deezer_charts_nulls,
  SUM(CASE WHEN in_shazam_charts IS NULL THEN 1 ELSE 0 END) AS in_shazam_charts_nulls,
FROM
  `laboratoria-projeto2-457801.projeto2.2`
```
```SQL
SELECT
  SUM(CASE WHEN track_id IS NULL THEN 1 ELSE 0 END) AS track_id_nulls,
  SUM(CASE WHEN bpm IS NULL THEN 1 ELSE 0 END) AS bpm_nulls,
  SUM(CASE WHEN key IS NULL THEN 1 ELSE 0 END) AS key_nulls,
  SUM(CASE WHEN mode IS NULL THEN 1 ELSE 0 END) AS mode_nulls,
  SUM(CASE WHEN danceability__ IS NULL THEN 1 ELSE 0 END) AS danceability___nulls,
  SUM(CASE WHEN valence__ IS NULL THEN 1 ELSE 0 END) AS valence___nulls,
  SUM(CASE WHEN energy__ IS NULL THEN 1 ELSE 0 END) AS energy___nulls,
  SUM(CASE WHEN acousticness__ IS NULL THEN 1 ELSE 0 END) AS acousticness___nulls,
  SUM(CASE WHEN instrumentalness__ IS NULL THEN 1 ELSE 0 END) AS instrumentalness___nulls,
  SUM(CASE WHEN liveness__ IS NULL THEN 1 ELSE 0 END) AS liveness___nulls,
  SUM(CASE WHEN speechiness__ IS NULL THEN 1 ELSE 0 END) AS speechiness___nulls,
FROM
  `laboratoria-projeto2-457801.projeto2.3`
```
```SQL
CREATE VIEW
  `laboratoria-projeto2-457801.projeto2.ViewNulos` AS
SELECT
  track_id,
  in_apple_playlists,
  in_apple_charts,
  in_deezer_playlists,
  in_deezer_charts,
  IFNULL(in_shazam_charts, -1) AS in_shazam_charts,
FROM
  `laboratoria-projeto2-457801.projeto2.2`



#SELECT * FROM `laboratoria-projeto2-457801.projeto2.ViewNulos` WHERE in_shazam_charts = -1

```
```SQL
CREATE VIEW
  `laboratoria-projeto2-457801.projeto2.ViewNulos2` AS
SELECT
  track_id,
  bpm,
  IFNULL(key, 'Não informado') AS key,
  mode,
  danceability__,
  valence__,
  energy__,
  acousticness__,
  instrumentalness__,
  liveness__,
  speechiness__
FROM
  `laboratoria-projeto2-457801.projeto2.3`



SELECT * FROM `laboratoria-projeto2-457801.projeto2.ViewNulos2` WHERE key = 'Não informado'
```
5.1.3 🔵 Identificar e tratar valores duplicados
```SQL
SELECT
  #track_id,
  track_name,
  artists_name,
  COUNT(*) AS quantidade
FROM
  `laboratoria-projeto2-457801.projeto2.1`
GROUP BY
  #track_id,
  track_name,
  artists_name
HAVING
  COUNT(*) > 1
```
```SQL
-- PARA VER AS LINHAS DUPLICADAS E TODAS AS COLUNAS - RESOLVI APAGAR UMA DE CADA 
-- POIS ESTAVA, IGUAIS

-- Subconsulta com os registros duplicados
WITH duplicados AS (
  SELECT 
    track_name,
    artists_name
  FROM `laboratoria-projeto2-457801.projeto2.1`
  GROUP BY track_name, artists_name
  HAVING COUNT(*) > 1
)

-- Consulta principal unindo com a subconsulta
SELECT *
FROM `laboratoria-projeto2-457801.projeto2.1` AS original
JOIN duplicados
ON original.track_name = duplicados.track_name
   AND original.artists_name = duplicados.artists_name
ORDER BY original.track_name, original.artists_name;

```
```SQL
SELECT
  track_id,
  COUNT(*) AS quantidade
FROM
  `laboratoria-projeto2-457801.projeto2.2`
GROUP BY
  track_id
HAVING
  COUNT(*) > 1
 
```
```SQL
SELECT
  track_id,
  COUNT(*) AS quantidade
FROM
  `laboratoria-projeto2-457801.projeto2.3`
GROUP BY
  track_id
HAVING
  COUNT(*) > 1
 
```
```SQL
CREATE VIEW
  `laboratoria-projeto2-457801.projeto2.ViewDuplicatas` AS
SELECT
  *,
  ROW_NUMBER() OVER (PARTITION BY track_name, artists_name ORDER BY track_name ) AS rn
FROM
  `laboratoria-projeto2-457801.projeto2.1`

#SELECT * FROM `laboratoria-projeto2-457801.projeto2.ViewDuplicatas` WHERE rn = 1;
```
```SQL
--eu dava o rn= na view e ele criava a coluna de duplicado ou original so na consulta
CREATE VIEW
  `laboratoria-projeto2-457801.projeto2.ViewDuplicatas` AS
SELECT
  *,
  ROW_NUMBER() OVER (PARTITION BY track_name, artists_name ORDER BY track_name ) AS rn
FROM
  `laboratoria-projeto2-457801.projeto2.1`

SELECT *, CASE WHEN rn = 1 THEN 'original' ELSE 'duplicata' END AS duplicata FROM `laboratoria-projeto2-457801.projeto2.ViewDuplicatas`
```
```SQL
--passei a fazer a propria view ja criar a coluna de original e duplicada
CREATE VIEW 
	`laboratoria-projeto2-457801.projeto2.ViewDuplicatas` AS
SELECT
  sub.track_id,
  sub.track_name,
  sub.artists_name,
  sub.artist_count,
  sub.released_year,
  sub.released_month,
  sub.released_day,
  sub.in_spotify_playlists,
  sub.in_spotify_charts,
  sub.streams
  CASE 
    WHEN sub.rn = 1 THEN 'original'
    ELSE 'duplicata'
  END AS duplicata
FROM (
  SELECT
    *,
    ROW_NUMBER() OVER (PARTITION BY track_name, artists_name ORDER BY track_name) AS rn
  FROM
    `laboratoria-projeto2-457801.projeto2.1`
) sub;
```
5.1.4 🔵 Identificar e tratar dados fora do escopo de análise
```SQL
SELECT * EXCEPT (key, mode) FROM `laboratoria-projeto2-457801.projeto2.3`
```
```SQL
CREATE VIEW
  `laboratoria-projeto2-457801.projeto2.ViewForaEscopo` AS
SELECT
  * EXCEPT (key,
    mode)
FROM
  `laboratoria-projeto2-457801.projeto2.ViewNulos2`


SELECT * FROM`laboratoria-projeto2-457801.projeto2.ViewForaEscopo`
```
5.1.5 🔵 Identificar e tratar dados discrepantes em variáveis categóricas
```SQL
SELECT track_id
FROM `laboratoria-projeto2-457801.projeto2.1`
WHERE NOT REGEXP_CONTAINS(track_id, r'^-?\d+(\.\d+)?$')
  AND track_id IS NOT NULL

```
```SQL
SELECT track_name
FROM `laboratoria-projeto2-457801.projeto2.1`
WHERE REGEXP_CONTAINS(track_name, r'^-?\d+(\.\d+)?$')
```
```SQL
SELECT streams
FROM laboratoria-projeto2-457801.projeto2.1
WHERE NOT REGEXP_CONTAINS(streams, r'^-?\d+(\.\d+)?$')
  AND streams IS NOT NULL
```
```SQL
-- retornar se tem
SELECT
  COUNT(*) > 0 AS has_lowercase_start
FROM
  `laboratoria-projeto2-457801.projeto2.1`
WHERE REGEXP_CONTAINS(track_name, r'^[a-z]')
  AND track_name IS NOT NULL

```
```SQL
-- retornar lista das minusculas 
SELECT track_name
FROM `laboratoria-projeto2-457801.projeto2.1`
WHERE REGEXP_CONTAINS(track_name, r'^[a-z]')
  AND track_name IS NOT NULL

```
```SQL
-- retornar se tem
SELECT
  COUNT(*) > 0 AS has_lowercase_start
FROM
  `laboratoria-projeto2-457801.projeto2.1`
WHERE REGEXP_CONTAINS(artists_name, r'^[a-z]')
  AND artists_name IS NOT NULL

```
```SQL
-- retornar lista das minusculas 
SELECT artists_name
FROM `laboratoria-projeto2-457801.projeto2.1`
WHERE REGEXP_CONTAINS(artists_name, r'^[a-z]')
  AND artists_name IS NOT NULL

```
```SQL
CREATE VIEW `laboratoria-projeto2-457801.projeto2.ViewCaps` AS
SELECT
  track_id,
  UPPER(track_name) AS track_name,
  UPPER(artists_name) AS artists_name,
  artist_count,
  released_year,
  released_month,
  released_day,
  in_spotify_playlists,
  in_spotify_charts,
  streams,
  duplicata,
FROM
  `laboratoria-projeto2-457801.projeto2.ViewDuplicatas`
WHERE
  (track_name, artists_name) IS NOT NULL

SELECT * FROM `laboratoria-projeto2-457801.projeto2.ViewCaps`

```
```SQL
CREATE VIEW
  `laboratoria-projeto2-457801.projeto2.ViewCaracter` AS
SELECT
  track_id,
  REGEXP_REPLACE(track_name, r'[�]', '') AS track_name,
  REGEXP_REPLACE(artists_name, r'[�]', '') AS artists_name,
  artist_count,
  released_year,
  released_month,
  released_day,
  in_spotify_playlists,
  in_spotify_charts,
  streams,
  duplicata 
FROM
  `laboratoria-projeto2-457801.projeto2.ViewCaps`



SELECT * FROM `laboratoria-projeto2-457801.projeto2.ViewCaracter`
```
5.1.6 🔵 Identificar e tratar dados discrepantes em variáveis numéricas
```SQL
CREATE VIEW
  `laboratoria-projeto2-457801.projeto2.ViewStreams` AS
SELECT
  track_id,
  track_name,
  artists_name,
  artist_count,
  released_year,
  released_month,
  released_day,
  in_spotify_playlists,
  in_spotify_charts,
  CASE
    WHEN streams = 'BPM110KeyAModeMajorDanceability53Valence75Energy69Acousticness7Instrumentalness0Liveness17Speechiness3' THEN '0'
    ELSE streams
END
  AS streams,
  duplicata
FROM
  `laboratoria-projeto2-457801.projeto2.ViewCaracter`

SELECT * FROM `laboratoria-projeto2-457801.projeto2.ViewStreams` WHERE streams = '0'#'BPM110KeyAModeMajorDanceability53Valence75Energy69Acousticness7Instrumentalness0Liveness17Speechiness3'
```
```SQL
SELECT
MAX(streams),
MIN(streams),
FROM `laboratoria-projeto2-457801.projeto2.ViewStreams`
```
```SQL
CREATE VIEW
  `laboratoria-projeto2-457801.projeto2.ViewTrackId` AS
SELECT
  CASE
    WHEN track_id = '00:00' THEN '0000000'
    ELSE track_id
END
  AS track_id,
  track_name,
  artists_name,
  artist_count,
  released_year,
  released_month,
  released_day,
  in_spotify_playlists,
  in_spotify_charts,
  streams,
  duplicata
FROM
  `laboratoria-projeto2-457801.projeto2.ViewStreams`

SELECT * FROM `laboratoria-projeto2-457801.projeto2.ViewTrackId` WHERE track_id = '0000000'
```
```SQL
SELECT
MAX(track_id),
MIN(track_id),
FROM `laboratoria-projeto2-457801.projeto2.ViewTrackId`
```
```SQL
SELECT
MAX(artist_count),
MIN(artist_count),
AVG(artist_count)
FROM `laboratoria-projeto2-457801.projeto2.1`
```
```SQL
SELECT
MAX(released_year),
MIN(released_year),
AVG(released_year)
FROM `laboratoria-projeto2-457801.projeto2.1`

```
```SQL
SELECT
MAX(in_spotify_playlists),
MIN(in_spotify_playlists),
AVG(in_spotify_playlists)
FROM `laboratoria-projeto2-457801.projeto2.1`

```
```SQL
SELECT
MAX(danceability__),
MIN(danceability__),
AVG(danceability__)
FROM `laboratoria-projeto2-457801.projeto2.3`

```
```SQL
SELECT
MAX(energy__),
MIN(energy__),
AVG(energy__)
FROM `laboratoria-projeto2-457801.projeto2.3`

```
```SQL
SELECT
MAX(instrumentalness__),
MIN(instrumentalness__),
AVG(instrumentalness__)
FROM `laboratoria-projeto2-457801.projeto2.3`
```
5.1.7 🔵 Verificar e alterar o tipo de dados
```SQL
CREATE VIEW `laboratoria-projeto2-457801.projeto2.ViewType` AS
SELECT
  track_id,
  track_name,
  artists_name,
  artist_count,
  DATE(released_year, released_month, released_day) AS data,
  in_spotify_playlists,
  in_spotify_charts,
  CAST(streams AS INT64) AS streams,
  duplicata
FROM
  `laboratoria-projeto2-457801.projeto2.ViewTrackId`


SELECT * FROM `laboratoria-projeto2-457801.projeto2.ViewType` 
```
5.1.8 🔵 Criar novas variáveis
```SQL
CREATE VIEW `laboratoria-projeto2-457801.projeto2.ViewVariavel` AS
SELECT 
  track_id,
  track_name,
  artists_name,
  artist_count,
  data,
  in_spotify_playlists,
  in_spotify_charts,
  streams,
  duplicata,
  SUM(in_spotify_playlists + in_spotify_charts) AS total_spotify,
FROM `laboratoria-projeto2-457801.projeto2.ViewType`
GROUP BY track_id, 
  track_name,
  artists_name,
  artist_count,
  DATA,
  in_spotify_playlists,
  in_spotify_charts,
  streams,
  duplicata

  SELECT * FROM `laboratoria-projeto2-457801.projeto2.ViewVariavel` 
```
5.1.9 🔵 Unir tabelas
```SQL
CREATE OR REPLACE TABLE `laboratoria-projeto2-457801.projeto2.Tabela1` AS
SELECT  
  track_id,
  track_name,
  artists_name,
  artist_count,
  data,
  in_spotify_playlists,
  in_spotify_charts,
  streams,
  duplicata,
  SUM(in_spotify_playlists + in_spotify_charts) AS total_spotify
FROM 
  `laboratoria-projeto2-457801.projeto2.ViewType`
GROUP BY 
  track_id, 
  track_name,
  artists_name,
  artist_count,
  data,
  in_spotify_playlists,
  in_spotify_charts,
  streams,
  duplicata;

```
```SQL
CREATE OR REPLACE TABLE `laboratoria-projeto2-457801.projeto2.Tabela2` AS
SELECT
  track_id,
  in_apple_playlists,
  in_apple_charts,
  in_deezer_playlists,
  in_deezer_charts,
  IFNULL(in_shazam_charts, -1) AS in_shazam_charts
FROM
  `laboratoria-projeto2-457801.projeto2.2`;
```
```SQL
CREATE OR REPLACE TABLE `laboratoria-projeto2-457801.projeto2.Tabela3` AS
SELECT
  * EXCEPT (key, mode)
FROM
  `laboratoria-projeto2-457801.projeto2.ViewNulos2`;
```
```SQL
CREATE OR REPLACE TABLE `laboratoria-projeto2-457801.projeto2.Tabela` AS
SELECT
  t1.*,
  t2.in_apple_playlists,
  t2.in_apple_charts,
  t2.in_deezer_playlists,
  t2.in_deezer_charts,
  t2.in_shazam_charts,
  t3.bpm,
  t3.danceability__,
  t3.valence__,
  t3.energy__,
  t3.acousticness__,
  t3.instrumentalness__,
  t3.liveness__,
  t3.speechiness__
FROM
  `laboratoria-projeto2-457801.projeto2.Tabela1` AS t1
LEFT JOIN `laboratoria-projeto2-457801.projeto2.Tabela2` AS t2
  ON t1.track_id = t2.track_id
LEFT JOIN `laboratoria-projeto2-457801.projeto2.Tabela3` AS t3
  ON t1.track_id = t3.track_id;

```
5.1.10 |🔵 Construir tabelas auxiliares
```SQL
WITH artista AS(
SELECT 
  artists_name,
  COUNT(*) AS total_musicas
  FROM `laboratoria-projeto2-457801.projeto2.Tabela`
  GROUP BY artists_name
)


SELECT
  t.track_id,
  t.track_name,
  t.artist_count,
  t.artists_name,
  a.total_musicas
FROM `laboratoria-projeto2-457801.projeto2.Tabela` t

LEFT JOIN artista AS a
ON t.artists_name = a.artists_name

```
5.2.7 🟣 Calcular quartis, decis ou percentis
```SQL
WITH quartil AS (
  SELECT  
    streams,
    NTILE(4) OVER (ORDER BY streams) AS quartil_streams
  FROM
    `laboratoria-projeto2-457801.projeto2.Tabela` 
)
SELECT 
t.*,
quartil.quartil_streams
FROM`laboratoria-projeto2-457801.projeto2.Tabela` t

LEFT JOIN quartil

ON t.streams=quartil.streams
```
```SQL
SELECT 
  *,
  NTILE(4) OVER (ORDER BY streams) AS quartil_streams,
  CASE 
    WHEN NTILE(4) OVER (ORDER BY streams) = 4 THEN 'alto'
    WHEN NTILE(4) OVER (ORDER BY streams) = 3 THEN 'médio'
    WHEN NTILE(4) OVER (ORDER BY streams) = 2 THEN 'baixo'
    ELSE 'muito baixo'
  END AS categoria_streams
FROM `laboratoria-projeto2-457801.projeto2.Tabela`

```
```SQL
CREATE OR REPLACE TABLE `laboratoria-projeto2-457801.projeto2.Tabela` AS
SELECT 
  *,
  NTILE(4) OVER (ORDER BY streams) AS quartil_streams,
  CASE 
    WHEN NTILE(4) OVER (ORDER BY streams) = 4 THEN 'alto'
    WHEN NTILE(4) OVER (ORDER BY streams) = 3 THEN 'médio'
    WHEN NTILE(4) OVER (ORDER BY streams) = 2 THEN 'baixo'
    ELSE 'muito baixo'
  END AS categoria_streams
FROM `laboratoria-projeto2-457801.projeto2.Tabela`
```
5.2.8 🟣 Calcular correlação entre variáveis
```SQL
SELECT 
CORR(streams, total_spotify) AS correlation
FROM `laboratoria-projeto2-457801.projeto2.Tabela`
0.79062773810857379(antes de duplicados e streams)
0.79062773810857412
0.79062773810857256



quase perto de 1 - tem uma correlacao linear - uma pode aumentar conforme a outra aumenta
e uma pode diminuir conforme a outra diminui

SELECT 
CORR(streams, danceability__) AS correlation
FROM `laboratoria-projeto2-457801.projeto2.Tabela`
-0.10438719371078554(antes de duplicados e streams)
-0.10523636796344503
-0.10530075541670146

0 - não tem nenhuma correlaçao - uma nao aumenta conforme a outra aumenta 
```
Validação de hipóteses
```SQL
WITH artista AS(
SELECT 
  artists_name,
  COUNT(*) AS total_musicas
  FROM `laboratoria-projeto2-457801.projeto2.Tabela`
  GROUP BY artists_name
)


SELECT
  t.track_id,
  t.track_name,
  t.artist_count,
  t.artists_name,
  a.total_musicas,
  t.streams
FROM `laboratoria-projeto2-457801.projeto2.Tabela` t

LEFT JOIN artista AS a
ON t.artists_name = a.artists_name



WITH artista AS(
SELECT 
  artists_name,
  COUNT(*) AS total_musicas
  FROM `laboratoria-projeto2-457801.projeto2.Tabela`
  GROUP BY artists_name
)

SELECT
  CORR(a.total_musicas, t.streams) AS correlacao
FROM
  `laboratoria-projeto2-457801.projeto2.Tabela` t
  
LEFT JOIN artista AS a
ON t.artists_name = a.artists_name;
```
```SQL
SELECT
  CORR(danceability__, streams) AS corr_danceability,
  CORR(energy__, streams) AS corr_energy,
  CORR(valence__, streams) AS corr_valence,
  CORR(acousticness__, streams) AS corr_acousticness,
  CORR(speechiness__, streams) AS corr_speechiness,
  CORR(instrumentalness__, streams) AS corr_instrumentalness,
  CORR(bpm, streams) AS corr_bpm,
  CORR(UNIX_DATE(data), streams) AS corr_data,
FROM `laboratoria-projeto2-457801.projeto2.Tabela`;

```
Pré processamento de dados, parte da String na coluna Streams
```SQL
CREATE OR REPLACE TABLE `laboratoria-projeto2-457801.projeto2.Tabela` AS
SELECT
  track_id,
  track_name,
  artists_name,
  artist_count,
  data,
  in_spotify_playlists,
  in_spotify_charts,
  total_spotify,
  in_apple_playlists,
  in_apple_charts,
  in_deezer_playlists,
  in_deezer_charts,
  in_shazam_charts,
  bpm,
  danceability__,
  valence__,
  energy__,
  acousticness__,
  instrumentalness__,
  liveness__,
  speechiness__,
  quartil_streams,
  categoria_streams,
  CASE
    WHEN EXTRACT(YEAR FROM data) = 1970 AND streams = 0 THEN 394968158
    ELSE streams
  END AS streams
FROM `laboratoria-projeto2-457801.projeto2.Tabela`;

```
