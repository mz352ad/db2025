SELECT k.nazov AS krajina, k.kod AS skratka, STRING_AGG(b.priezvisko, ',' ORDER BY b.priezvisko ASC) AS bezci, COUNT(b.id_bezec) AS pocet
FROM krajina k
JOIN bezec b ON k.id_krajina = b.id_krajina
GROUP BY k.nazov, k.kod
ORDER BY k.nazov ASC, k.kod ASC, pocet DESC;


SELECT b.meno AS meno, b.priezvisko AS priezvisko, k.nazov AS stat
FROM bezec b
JOIN krajina k ON b.id_krajina = k.id_krajina
WHERE k.kontinent = 'Európa' AND k.nazov != 'Česko'
ORDER BY meno ASC, priezvisko DESC, stat ASC;


SELECT p.nazov AS pretek
FROM bezec b
JOIN bezec_pretek bp ON b.id_bezec = bp.id_bezec
JOIN pretek p ON bp.id_pretek = p.id_pretek
WHERE b.meno = 'Marek' AND b.priezvisko = 'Kováč'
ORDER BY pretek ASC;


SELECT b.meno AS meno, b.priezvisko AS priezvisko, p.nazov AS pretek
FROM bezec b
JOIN bezec_pretek bp ON b.id_bezec = bp.id_bezec
JOIN pretek p ON p.id_pretek = bp.id_pretek
WHERE EXTRACT(YEAR FROM p.datum) = 2025 AND EXTRACT(MONTH FROM p.datum) = 5
ORDER BY meno ASC, priezvisko ASC, pretek ASC;


SELECT k.kod AS krajina, b.stredne_meno AS bezec, COUNT(bp.id_pretek) AS dokoncili
FROM bezec b
JOIN krajina k ON b.id_krajina = k.id_krajina
JOIN bezec_pretek bp ON b.id_bezec = bp.id_bezec
WHERE b.stredne_meno IS NOT NULL AND bp.cas_v_milisekundach IS NOT NULL AND k.kod LIKE '%R%'
GROUP BY k.kod, b.stredne_meno
ORDER BY krajina ASC, bezec ASC, dokoncili DESC;


SELECT k.kontinent AS kontinent, COUNT(*) AS pocet_bezcov
FROM bezec b
JOIN krajina k ON b.id_krajina = k.id_krajina
JOIN bezec_pretek bp ON b.id_bezec = bp.id_bezec
WHERE bp.cas_v_milisekundach < 140000 AND k.nazov != 'India' AND b.datum_narodenia >= '1985-07-21'
GROUP BY k.kontinent
HAVING COUNT(*) > 6
ORDER BY pocet_bezcov DESC, kontinent ASC;


SELECT b.meno || ' ' || b.priezvisko AS meno, bp.cas_v_milisekundach / 1000 AS cas, p.nazov AS pretek
FROM bezec b
JOIN bezec_pretek bp ON b.id_bezec = bp.id_bezec
JOIN pretek p ON bp.id_pretek = p.id_pretek
JOIN krajina k ON b.id_krajina = k.id_krajina
WHERE bp.cas_v_milisekundach IS NOT NULL AND b.stredne_meno IS NULL AND k.rozloha_v_km2 <= 400000
ORDER BY pretek ASC, cas ASC, meno ASC;


SELECT b.meno AS meno, b.priezvisko AS priezvisko, k.nazov AS stat
FROM bezec b
JOIN krajina k ON b.id_krajina = k.id_krajina
WHERE k.kontinent != 'Európa'
ORDER BY stat ASC, priezvisko ASC, meno ASC;


SELECT p.nazov AS nazov, p.dlzka_v_metroch AS dlzka, bp.cas_v_milisekundach AS cas
FROM pretek p
JOIN bezec_pretek bp ON p.id_pretek = bp.id_pretek
WHERE bp.cas_v_milisekundach IS NOT NULL AND p.nazov = 'Taliansky polmaratón'
ORDER BY nazov ASC, dlzka ASC, cas ASC;


SELECT k.nazov AS stat, CEIL(AVG(bp.cas_v_milisekundach) / 1000) AS primer, COUNT(DISTINCT b.id_bezec) AS pocet
FROM bezec b
JOIN bezec_pretek bp on b.id_bezec = bp.id_bezec
JOIN krajina k ON b.id_krajina = k.id_krajina
WHERE bp.cas_v_milisekundach IS NOT NULL
GROUP BY k.nazov
ORDER BY primer ASC, pocet ASC;


SELECT p.nazov AS pretek, EXTRACT(YEAR FROM p.datum) AS rok, p.dlzka_v_metroch AS dlzka
FROM pretek p
WHERE p.dlzka_v_metroch BETWEEN 40000 AND 50000
ORDER BY pretek ASC, rok ASC, dlzka DESC;


SELECT b.meno || b.priezvisko AS meno, bp.cas_v_milisekundach / 1000 AS cas
FROM bezec b
JOIN bezec_pretek bp ON b.id_bezec = bp.id_bezec
JOIN pretek p ON bp.id_pretek = p.id_pretek
WHERE p.nazov = 'Maratón v Berlíne'
ORDER BY cas ASC, b.priezvisko ASC, b.meno ASC;


SELECT b.meno AS meno, b.priezvisko AS priezvisko, p.nazov AS pretek, bp.cas_v_milisekundach AS milisekundy
FROM bezec b
JOIN bezec_pretek bp ON b.id_bezec = bp.id_bezec
JOIN pretek p ON bp.id_pretek = p.id_pretek
WHERE p.nazov = 'Beh po Alpách'
ORDER BY meno ASC, priezvisko ASC, pretek ASC, milisekundy DESC;


SELECT b.meno AS meno, b. priezvisko AS priezvisko, k.nazov AS stat, k.rok_vzniku AS vznik
FROM bezec b
JOIN krajina k ON b.id_krajina = k.id_krajina
WHERE k.kod IN ('IND', 'USA') AND b.stredne_meno IS NOT NULL
ORDER BY meno ASC, priezvisko ASC, stat ASC, vznik ASC;

SELECT p.nazov AS pretek, COUNT(bp.id_bezec) AS pocet
FROM pretek p
JOIN bezec_pretek bp on p.id_pretek = bp.id_pretek
GROUP BY p.nazov
ORDER BY pocet DESC, pretek DESC;


SELECT b.meno AS meno, b.priezvisko AS priezvisko, EXTRACT(YEAR FROM b.datum_narodenia) AS rok_narodenia
FROM bezec b
WHERE LENGTH(b.priezvisko) >= 5 AND SUBSTRING(b.priezvisko FROM 2 FOR 1) = 'o' AND SUBSTRING(b.priezvisko FROM (LENGTH(b.priezvisko) - 1) FOR 1) = 'e'
ORDER BY meno ASC, priezvisko ASC, rok_narodenia DESC;


SELECT b.meno || ' ' || b.priezvisko AS ucastnik, k.Rozloha_v_km2 AS rozloha, CEIL(AVG(bp.cas_v_milisekundach)) AS cas
FROM bezec b
JOIN krajina k ON b.id_krajina = k.id_krajina
JOIN bezec_pretek bp ON b.id_bezec = bp.id_bezec
WHERE bp.cas_v_milisekundach IS NOT NULL AND b.priezvisko LIKE '%a'
GROUP BY b.meno, b.priezvisko, k.Rozloha_v_km2
ORDER BY cas DESC, rozloha ASC, ucastnik ASC;


SELECT p.nazov AS pretek, p.datum AS datum, COUNT(bp.id_bezec) AS bezcov, COUNT(DISTINCT k.id_krajina) AS krajin
FROM pretek p
JOIN bezec_pretek bp ON p.id_pretek = bp.id_pretek
JOIN bezec b ON bp.id_bezec = b.id_bezec
JOIN krajina k ON b.id_krajina = k.id_krajina
WHERE LOWER(p.nazov) like '%maratón%'
GROUP BY p.nazov, p.datum
HAVING COUNT(*) < 15
ORDER BY bezcov DESC, krajin DESC, pretek ASC, datum ASC;


SELECT b.priezvisko AS priezvisko, ROUND(AVG(bp.cas_v_milisekundach) / 1000, 2) AS priemer, COUNT(bp.id_pretek) AS pocet
FROM bezec b
JOIN bezec_pretek bp ON b.id_bezec = bp.id_bezec
JOIN pretek p ON bp.id_pretek = p.id_pretek
JOIN krajina k ON b.id_krajina = k.id_krajina
WHERE b.datum_narodenia >= '1995-01-01' AND p.nazov != 'Polmaratón v Prahe' AND k.kontinent = 'Európa' AND bp.cas_v_milisekundach IS NOT NULL
GROUP BY b.priezvisko
ORDER BY priemer ASC, pocet ASC;


SELECT k.nazov AS stat, COUNT(b.id_bezec) AS pocet
FROM krajina k
JOIN bezec b ON k.id_krajina = b.id_krajina
GROUP BY k.nazov
ORDER BY pocet ASC, stat ASC;
