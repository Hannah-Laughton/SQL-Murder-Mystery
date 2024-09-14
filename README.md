# SQL

SQL Code
--SQL Murder Mystery

--Step 1:
--Murder
--Occurred Jan.15, 2018
--Took place in SQL City
--Start by retrieving the corresponding crime scene report from the police department’s database

SELECT *
FROM crime_scene_report
WHERE type = 'murder'
AND date = '20180115'
AND city = 'SQL City'

---

--Step 2:
--2 witnesses
--First lives at the last house on "Northwestern Dr"
--Second, named Annabel, lives somewhere on "Franklin Ave"

--Witness 1

SELECT *
FROM person
WHERE address_street_name = 'Northwestern Dr'
ORDER BY address_number DESC
LIMIT 1

--Witness 2

SELECT *
FROM person
WHERE address_street_name = 'Franklin Ave'
AND name LIKE 'Annabel%'

---

--Step 3:
--Witness 1: id (14887), name (Morty Schapiro), license_id (118009), address_number (4919), address_street_name (Northwestern Dr), ssn (111564949)
--Witness 2: id (16371), name (Annabel Miller), license_id (490173), address_number (103), address_street_name (Franklin Ave), ssn (318771143)

SELECT p.id, p.name, i.transcript
FROM person AS p
JOIN interview AS i
ON p.id = i.person_id
WHERE p.id IN (14887,16371)

---

--Step 4:
--Witness 1 (Morty Schapiro): I heard a gunshot and then saw a man run out. He had a "Get Fit Now Gym" bag. The membership number on the bag started with "48Z". Only gold members have those bags. The man got into a car with a plate that included "H42W".
--Witness 2 (Annabel Miller): I saw the murder happen, and I recognized the killer from my gym when I was working out last week on January the 9th.

--Suspect

SELECT c.check_in_date, m.id, m.name, m.membership_status
FROM get_fit_now_check_in AS c
JOIN get_fit_now_member AS m
ON c.membership_id = m.id
JOIN person AS p
ON m.person_id = p.id
WHERE m.id LIKE '48Z%'
AND m.membership_status = 'gold'

--Driver

SELECT *
FROM drivers_license
WHERE plate_number LIKE '%H42W%'

---

--Step 5:
--Suspect 1 (Joe Germuska): check_in_date (20180109), id (48Z7A), name (Joe Germuska), membership_status (gold)
--Suspect 2 (Jeremy Bowers): check_in_date (20180109), id (48Z55), name (Jeremy Bowers), membership_status (gold)
--Driver 1: id (183779), age (21), height (65), eye_color (blue), hair_color (blonde), gender (female), plate_number (H42W0X), car_make (Toyota), car_model (Prius)
--Driver 2: id (423327), age (30), height (70), eye_color (brown), hair_color (brown), gender (male), plate_number (0H42W2), car_make (Chevrolet), car_model (Spark LS)
--Driver 3: id (664760), age (21), height (71), eye_color (black), hair_color (black), gender (male), plate_number (4H42WR), car_make (Nissan), car_model (Altima)

SELECT d.*, p.name, i.person_id, i.transcript
FROM drivers_license AS d
JOIN person AS p
ON d.id = p.license_id
JOIN interview AS i
ON p.id = i.person_id
WHERE d.plate_number LIKE '%H42W%'

---

--Step 6:
--Jeremy Bowers (Murderer): I was hired by a woman with a lot of money. I don't know her name but I know she's around 5'5" (65") or 5'7" (67"). She has red hair and she drives a Tesla Model S. I know that she attended the SQL Symphony Concert 3 times in December 2017.

SELECT *
FROM drivers_license AS d
JOIN person AS p
ON d.id = p.license_id
JOIN income AS i
ON p.ssn = i.ssn
JOIN facebook_event_checkin AS F
ON p.id = f.person_id
AND d.hair_color = 'red'
AND d.car_make = 'Tesla'
WHERE f.event_name = 'SQL Symphony Concert'
ORDER BY i.annual_income DESC

---

--Step 7:
--Murderer: Jeremy Bowers
--Villain: Miranda Priestly
