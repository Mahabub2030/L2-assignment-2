# L2-assignment-2
🏗️ 1. Table Creation (with constraints)
Create the following 3 tables:

-- Table: rangers
CREATE TABLE rangers (
  ranger_id SERIAL PRIMARY KEY,
  name VARCHAR(100) NOT NULL,
  region VARCHAR(100) NOT NULL
);

-- Table: species
CREATE TABLE species (
  species_id SERIAL PRIMARY KEY,
  common_name VARCHAR(100) NOT NULL,
  scientific_name VARCHAR(100),
  discovery_date DATE,
  conservation_status VARCHAR(50)
);

-- Table: sightings
CREATE TABLE sightings (
  sighting_id SERIAL PRIMARY KEY,
  species_id INT REFERENCES species(species_id),
  ranger_id INT REFERENCES rangers(ranger_id),
  sighting_time TIMESTAMP,
  location VARCHAR(100),
  notes TEXT
);
📥 2. Sample Data Insertion
Insert the data provided in the assignment into the 3 tables.

📌 3. SQL Queries for 9 Problems

-- Problem 1: Register a new ranger
INSERT INTO rangers (name, region)
VALUES ('Derek Fox', 'Coastal Plains');

-- Problem 2: Count unique species ever sighted
SELECT COUNT(DISTINCT species_id) AS unique_species_count
FROM sightings;

-- Problem 3: Sightings where location includes "Pass"
SELECT * FROM sightings
WHERE location ILIKE '%Pass%';

-- Problem 4: Ranger name with total sightings
SELECT r.name, COUNT(s.sighting_id) AS total_sightings
FROM rangers r
LEFT JOIN sightings s ON r.ranger_id = s.ranger_id
GROUP BY r.name;

-- Problem 5: Species never sighted
SELECT common_name
FROM species
WHERE species_id NOT IN (SELECT DISTINCT species_id FROM sightings);

-- Problem 6: Most recent 2 sightings
SELECT sp.common_name, s.sighting_time, r.name
FROM sightings s
JOIN species sp ON s.species_id = sp.species_id
JOIN rangers r ON s.ranger_id = r.ranger_id
ORDER BY s.sighting_time DESC
LIMIT 2;

-- Problem 7: Update species before 1800 to "Historic"
UPDATE species
SET conservation_status = 'Historic'
WHERE discovery_date < '1800-01-01';

-- Problem 8: Time of day label
SELECT sighting_id,
  CASE
    WHEN EXTRACT(HOUR FROM sighting_time) < 12 THEN 'Morning'
    WHEN EXTRACT(HOUR FROM sighting_time) BETWEEN 12 AND 17 THEN 'Afternoon'
    ELSE 'Evening'
  END AS time_of_day
FROM sightings;

-- Problem 9: Delete rangers with no sightings
DELETE FROM rangers
WHERE ranger_id NOT IN (
  SELECT DISTINCT ranger_id FROM sightings
);
📚 README Bonus Section (Any 5 in Bangla)
Examples:

1️⃣ PostgreSQL কী?
PostgreSQL একটি ওপেন সোর্স রিলেশনাল ডেটাবেইজ ম্যানেজমেন্ট সিস্টেম (RDBMS), যা ডেটা সংরক্ষণ, রিট্রাইভ এবং ম্যানিপুলেট করার জন্য ব্যবহৃত হয়। এটি SQL এবং আধুনিক ফিচার যেমন JSON, indexing, concurrency, stored procedures ইত্যাদি সাপোর্ট করে।

2️⃣ Primary Key এবং Foreign Key কী?
Primary Key হল একটি ইউনিক কলাম যা প্রতিটি সারিকে ইউনিকভাবে সনাক্ত করে।

Foreign Key এমন একটি কলাম যা অন্য টেবিলের Primary Key কে রেফারেন্স করে দুইটি টেবিলের মধ্যে সম্পর্ক স্থাপন করে।

[→ Continue for 3 more questions in similar style.]

🗂️ Suggested File Structure

B5A2/
├── PostgreSQL_Assignment.sql
└── README.md