# Traccia esercitazione n. 51:

Utilizzando lo stesso database di ieri, eseguite le query in allegato.
Caricate un secondo file nella stessa repo di ieri (db-university) con le query di oggi.

- Query con Group by
Contare quanti iscritti ci sono stati ogni anno
Contare gli insegnanti che hanno l'ufficio nello stesso edificio
Calcolare la media dei voti di ogni appello d'esame
Contare quanti corsi di laurea ci sono per ogni dipartimento

- Query con Join
Selezionare tutti gli studenti iscritti al Corso di Laurea in Economia
Selezionare tutti i Corsi di Laurea del Dipartimento di Neuroscienze
Selezionare tutti i corsi in cui insegna Fulvio Amato (id=44)
Selezionare tutti gli studenti con i dati relativi al corso di laurea a cui sono iscritti e il relativo dipartimento, in ordine alfabetico per cognome e nome
Selezionare tutti i corsi di laurea con i relativi corsi e insegnanti
Selezionare tutti i docenti che insegnano nel Dipartimento di Matematica (54)

- BONUS: Selezionare per ogni studente quanti tentativi d’esame ha sostenuto per superare ciascuno dei suoi esami

# Query
## Query con Group by

1. Contare quanti iscritti ci sono stati ogni anno
SELECT COUNT(id) AS `students_number`, YEAR(`enrolment_date`) AS `enrolment_year` 
FROM `students` 
GROUP BY `enrolment_year`;

2. Contare gli insegnanti che hanno l'ufficio nello stesso edificio
SELECT COUNT(*) AS `teachers_number`, `office_address` 
FROM `teachers` 
GROUP BY `office_address`;

3. Calcolare la media dei voti di ogni appello d'esame
SELECT  `exam_id`, AVG(`vote`) AS `average_votes`
FROM `exam_student` 
GROUP BY `exam_id`;    

4. Contare quanti corsi di laurea ci sono per ogni dipartimento
SELECT `department_id`, COUNT(*) AS `number_courses`
FROM `degrees` 
GROUP BY `department_id`;

## Query con Join

1. Selezionare tutti gli studenti iscritti al Corso di Laurea in Economia
SELECT `students`.`*`, `degrees`.`name` AS `name_course_degrees` 
FROM `students` 
JOIN `degrees` ON `students`.`degree_id` = `degrees`.`id` 
WHERE `degrees`.`name` = 'Corso di Laurea in Economia';

2. Selezionare tutti i Corsi di Laurea del Dipartimento di Neuroscienze
SELECT `degrees`.`*`, `departments`.`name` AS `department_name` 
FROM `departments` 
JOIN `degrees` ON `degrees`.`department_id` = `departments`.`id` 
WHERE `departments`.`name` = 'Dipartimento di Neuroscienze';

3. Selezionare tutti i corsi in cui insegna Fulvio Amato (id=44)
SELECT `courses`.`id` AS `course_id`, `teachers`.`name` AS `teacher_name`, `courses`.`name` AS `course_name`, `courses`.`description` AS `course_description`, `courses`.`period`, `courses`.`cfu`
FROM `teachers` 
JOIN `course_teacher` ON `teachers`.id = `course_teacher`.`teacher_id` JOIN `courses` ON `course_teacher`.`course_id` = `courses`.`id` 
WHERE `teachers`.`id` = '44'

4. Selezionare tutti gli studenti con i dati relativi al corso di laurea a cui sono iscritti e il relativo dipartimento, in ordine alfabetico per cognome e nome
SELECT `students`.`*`, `degrees`.`name` AS `degree_name`, `departments`.`name` AS `department_name`
FROM `students` 
JOIN `degrees` ON `students`.`id` = `degrees`.`id` 
JOIN `departments` ON `degrees`.`department_id` = `departments`.`id` 
ORDER BY `students`.`name`, `students`.`surname`;

5. Selezionare tutti i corsi di laurea con i relativi corsi e insegnanti
SELECT `degrees`.`id` AS `degrees_id`, `teachers`.`name` AS `teacher_name`, `teachers`.`surname` AS `teacher_surname`, `degrees`.`name` AS `degree_name`, `courses`.`name` AS `course_name`, `teachers`.`phone` AS `teachers_phone`, `teachers`.`email` AS `teachers_email`,  `teachers`.`office_address` AS `teachers_office_address`, `teachers`.`office_number` AS `teachers_office_number` 
FROM `degrees`
JOIN `courses` ON `degrees`.`id` = `courses`.`degree_id`
JOIN `course_teacher` ON `course_teacher`.`course_id` = `courses`.`id`
JOIN `teachers` ON `teachers`.`id` = `course_teacher`.`teacher_id`
ORDER BY `degrees`.`name`;

6. Selezionare tutti i docenti che insegnano nel Dipartimento di Matematica (54)
SELECT DISTINCT `teachers`.`id` AS `teacher_id`, `teachers`.`name` AS `teacher_name`, `teachers`.`surname` AS `teacher_surname`, `departments`.`name` AS `department_name`, `teachers`.`phone` AS `teachers_phone`, `teachers`.`email` AS `teachers_email`,  `teachers`.`office_address` AS `teachers_office_address`, `teachers`.`office_number` AS `teachers_office_number`
FROM `teachers`
JOIN `course_teacher` ON `teachers`.`id` = `course_teacher`.`teacher_id`
JOIN `courses` ON `course_teacher`.`course_id` = `courses`.`id`
JOIN `degrees` ON `courses`.`degree_id` = `degrees`.`id`
JOIN `departments` ON `degrees`.`department_id` = `departments`.`id`
WHERE `departments`.`name` = 'Dipartimento di Matematica';
