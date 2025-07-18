# Esercizio "Database università"

## Traccia del giorno 1

Modellizzare la struttura di un database, per memorizzare tutti i dati riguardanti un'università, tenendo conto che:

1. sono presenti diversi `Dipartimenti` (es.: Lettere e Filosofia, Matematica, Ingegneria ecc.);

2. ogni `Dipartimento` offre più `Corsi di Laurea` (es.: Civiltà e Letterature Classiche, Informatica, Ingegneria Elettronica ecc.);

3. ogni `Corso di Laurea` prevede diversi `Corsi` (es.: Letteratura Latina, Sistemi Operativi 1, Analisi Matematica 2 ecc.);

4. ogni `Corso` può essere tenuto da diversi `Insegnanti`;

5. ogni `Corso` prevede più appelli d'`Esame`;

6. ogni `Studente` è iscritto ad un solo `Corso di Laurea`;

7. ogni `Studente` può iscriversi a più appelli di `Esame`;

8. per ogni appello d'`Esame` a cui lo `Studente` ha partecipato è necessario memorizzare il voto ottenuto, anche se non sufficiente.

## Passaggi

1. Definire quali entità (tabelle) creare per il nostro database e stabilirne le relazioni.

2. Definire le colonne e i tipi di dato di ogni tabella.

3. Utilizzare https://www.drawio.com/ per la creazione dello schema.

4. Esportare il diagramma in formato .png caricarlo nel repository.

## Svolgimento

### Entities

- Departement

- Degree course

- Course

- Teacher

- Exam

- Student

### Relationships

- [2] Department     1:N Degree courses
 
- [3] Degree course  1:N Courses
 
- [4] Course         N:N Teachers
 
- [5] Course         1:N Exam

- [6] Degree course  1:N Student

- [7] Student        N:N Exam

### Tables

```sql
departements
    - id                       BIGINT       UNSIGNED NOT NULL UNIQUE AUTO_INCREMENT PRIMARY KEY
    - name                     VARCHAR(100)          NOT NULL
    - physical_address         VARCHAR(255)          NOT NULL
    - email_address            VARCHAR(255)          NOT NULL
    - telephone_number         VARCHAR(15)               NULL
    - website                  VARCHAR(255)              NULL
```

```sql
degree_courses
    - id                       BIGINT       UNSIGNED NOT NULL UNIQUE AUTO_INCREMENT PRIMARY KEY
    - department_id            FOREIGN KEY
    - name                     VARCHAR(150)          NOT NULL
    - description              VARCHAR(255)          NOT NULL
    - level                    VARCHAR(15)           NOT NULL
    - website                  VARCHAR(255)              NULL
```

```sql
courses
    - id                       BIGINT       UNSIGNED NOT NULL UNIQUE AUTO_INCREMENT PRIMARY KEY
    - degree_course_id         FOREIGN KEY
    - name                     VARCHAR(150)          NOT NULL
    - description              VARCHAR(255)          NOT NULL
    - credits_number           TINYINT      UNSIGNED NOT NULL
    - year                     TINYINT      UNSIGNED NOT NULL
    - semester                 TINYINT      UNSIGNED NOT NULL
    - website                  VARCHAR(255)              NULL
```

```sql
course_teacher
    - course_id                FOREIGN KEY
    - teacher_id               FOREIGN KEY
    - (course_id + teacher_id) PRIMARY KEY
```

```sql
teachers
    - id                       BIGINT       UNSIGNED NOT NULL UNIQUE AUTO_INCREMENT PRIMARY KEY
    - first_name               VARCHAR(50)           NOT NULL
    - last_name                VARCHAR(50)           NOT NULL
    - email_address            VARCHAR(255)          NOT NULL
    - telephone_number         VARCHAR(15)               NULL
    - office_number            SMALLINT     UNSIGNED     NULL
```

```sql
exams
    - id                       BIGINT       UNSIGNED NOT NULL UNIQUE AUTO_INCREMENT PRIMARY KEY
    - course_id                FOREIGN KEY
    - date                     DATE                  NOT NULL
    - time                     TIME                  NOT NULL
    - location                 VARCHAR(255)          NOT NULL
```

```sql
exam_student
    - exam_id                  FOREIGN KEY
    - student_id               FOREIGN KEY
    - (exam_id + student_id)   PRIMARY KEY
    - vote                     TINYINT      UNSIGNED NOT NULL
```

```sql
students
    - id                       BIGINT       UNSIGNED NOT NULL UNIQUE AUTO_INCREMENT PRIMARY KEY
    - degree_course_id         FOREIGN KEY
    - student_code             VARCHAR(15)           NOT NULL
    - first_name               VARCHAR(50)           NOT NULL
    - last_name                VARCHAR(50)           NOT NULL
    - email_address            VARCHAR(255)          NOT NULL
    - enrollment_date          DATE                  NOT NULL
```



## Traccia del giorno 2

Risolvere le query basandosi sul database ottenuto dal file `schema.sql`.

## Svolgimento

1. Selezionare tutti gli studenti nati nel 1990. (160)

    ```sql
    SELECT *
    FROM `students`
    WHERE YEAR(`date_of_birth`) = 1990;
    ```

2. Selezionare tutti i corsi che valgono più di 10 crediti. (479)

    ```sql
    SELECT *
    FROM `courses`
    WHERE `cfu` > 10;
    ```

3. Selezionare tutti gli studenti che hanno più di 30 anni.

    ```sql
    SELECT *
    FROM `students`
    WHERE `date_of_birth` <= CURDATE() - INTERVAL 30 YEAR;
    ```

4. Selezionare tutti i corsi del primo semestre del primo anno di un qualsiasi corso di laurea. (286)

    ```sql
    SELECT *
    FROM `courses`
    WHERE `period` = 'I semestre' AND `year` = 1;
    ```

5. Selezionare tutti gli appelli d'esame che avvengono nel pomeriggio (dopo le 14) del 20/06/2020. (21)

    ```sql
    SELECT *
    FROM `exams`
    WHERE HOUR(`hour`) >= 14 AND DATE = '2020-06-20';
    ```

6. Selezionare tutti i corsi di laurea magistrale. (38)

    ```sql
    SELECT *
    FROM `degrees`
    WHERE `level` = 'magistrale';
    ```

7. Da quanti dipartimenti è composta l'università? (12)

    ```sql
    SELECT COUNT(*) AS departments_number
    FROM `departments`;
    ```

8. Quanti sono gli insegnanti che non hanno un numero di telefono? (50)

    ```sql
    SELECT COUNT(*) AS no_telephone_number
    FROM `teachers`
    WHERE `phone` IS NULL;
    ```



## Traccia del giorno 3

Risolvere le query basandosi sul database ottenuto dal file `schema.sql`.

## Svolgimento

### Group by

1. Contare quanti iscritti ci sono stati ogni anno.

    ```sql
    SELECT YEAR(`enrolment_date`) AS year, COUNT(YEAR(`enrolment_date`)) AS `enrollments_number`
    FROM `students`
    GROUP BY `year`;
    ```

2. Contare gli insegnanti che hanno l'ufficio nello stesso edificio.

    ```sql
    SELECT `office_address`, COUNT(`office_address`) AS `teachers_number`
    FROM `teachers`
    GROUP BY office_address;
    ```

3. Calcolare la media dei voti di ogni appello d'esame.

    ```sql
    SELECT `exam_id`, ROUND(AVG(`vote`)) AS `vote_average`
    FROM `exam_student`
    GROUP BY `exam_id`;
    ```

4. Contare quanti corsi di laurea ci sono per ogni dipartimento.

    ```sql
    SELECT `department_id`, COUNT(*) as `degree_courses_number`
    FROM `degrees`
    GROUP BY `department_id`;
    ```

### Join

1. Selezionare tutti gli studenti iscritti al Corso di Laurea in Economia.

    ```sql
    SELECT `students`.*, `degrees`.`name` AS `degree_course`
    FROM `students`
    JOIN `degrees` ON `degree_id` = `degrees`.`id`
    WHERE `degrees`.`name` = 'Corso di Laurea in Economia';
    ```

2. Selezionare tutti i Corsi di Laurea Magistrale del Dipartimento di Neuroscienze.

    ```sql
    SELECT `degrees`.*, `departments`.name AS 'department_name'
    FROM `degrees`
    JOIN `departments` ON `degrees`.`department_id` = `departments`.`id`
    WHERE `degrees`.`level` = 'magistrale' AND `departments`.`name` = 'Dipartimento di Neuroscienze';
    ```

3. Selezionare tutti i corsi in cui insegna Fulvio Amato. (id=44)

    ```sql
    SELECT `courses`.*, `course_teacher`.`teacher_id`
    FROM `course_teacher`
    JOIN `courses` ON `course_teacher`.`course_id` = `courses`.`id`
    WHERE `course_teacher`.`teacher_id` = 44;
    ```

4. Selezionare tutti gli studenti con i dati relativi al corso di laurea a cui sono iscritti e il relativo dipartimento, in ordine alfabetico per cognome e nome.

    ```sql
    SELECT
        -- Student data
        `students`.`id` AS `student_id`,
        `students`.`degree_id` AS `student_degree_id`,
        `students`.`surname` AS `student_surname`,
        `students`.`name` AS `student_name`,
        `students`.`date_of_birth` AS `student_date_of_birth`,
        `students`.`fiscal_code` AS `student_fiscal_code`,
        `students`.`enrolment_date` AS `student_enrolment_date`,
        `students`.`registration_number` AS `student_registration_number`,
        `students`.`email` AS `student_email`,
        -- Degree course data
        `degrees`.`id` AS `degree_id`,
        `degrees`.`department_id`,
        `degrees`.`name` AS `degree_name`,
        `degrees`.`level` AS `degree_level`,
        `degrees`.`address` AS `degree_address`,
        `degrees`.`email` AS `degree_email`,
        `degrees`.`website` AS `degree_website`,
        -- Department data
        `departments`.`name` AS `department`
    FROM `students`
    JOIN `degrees` ON `students`.`degree_id` = `degrees`.`id`
    JOIN `departments` ON `degrees`.`department_id` = `departments`.`id`
    ORDER BY `students`.`surname`, `students`.`name`;
    ```

5. Selezionare tutti i corsi di laurea con i relativi corsi e insegnanti.

    ```sql
    SELECT
        -- Degree course data
        `degrees`.`id` AS `degree_id`,
        `degrees`.`department_id`,
        `degrees`.`name` AS `degree_name`,
        `degrees`.`level` AS `degree_level`,
        `degrees`.`address` AS `degree_address`,
        `degrees`.`email` AS `degree_email`,
        `degrees`.`website` AS `degree_website`,
        -- Courses data
        `courses`.`id` AS `course_id`,
        `courses`.`degree_id`,
        `courses`.`name` AS `course_name`,
        `courses`.`description` AS `course_description`,
        `courses`.`period` AS `course_period`,
        `courses`.`year` AS `course_year`,
        `courses`.`cfu` AS `course_cfu`,
        `courses`.`website` AS `course_website`,
        -- Teachers data
        `teachers`.`id` AS `teacher_id`,
        `teachers`.`name`AS `teacher_name`,
        `teachers`.`surname` AS `teacher_surname`,
        `teachers`.`phone` AS `teacher_phone`,
        `teachers`.`email` AS `teacher_email`,
        `teachers`.`office_address` AS `teacher_office_address`,
        `teachers`.`office_number` AS `teacher_office_number`
    FROM `degrees`
    JOIN `courses` ON `degrees`.`id` = `courses`.`degree_id`
    JOIN `course_teacher` ON `courses`.`id` = `course_teacher`.`course_id`
    JOIN `teachers` ON `course_teacher`.`teacher_id` = `teachers`.`id`
    ORDER BY `degrees`.`id`, `courses`.`id`;
    ```

6. Selezionare tutti i docenti che insegnano nel Dipartimento di Matematica. (5)

    ```sql
    SELECT DISTINCT `teachers`.*, `degrees`.`department_id`
    FROM `teachers`
    JOIN `course_teacher` ON `teachers`.`id` = `course_teacher`.`teacher_id`
    JOIN `courses` ON `course_teacher`.`course_id` = `courses`.`id`
    JOIN `degrees` ON `courses`.`degree_id` = `degrees`.`id`
    WHERE `degrees`.`department_id` = 5;

    ```

7. **(BONUS):** Selezionare per ogni studente il numero di tentativi sostenuti per ogni esame, stampando anche il voto massimo. Successivamente, filtrare i tentativi con voto minimo 18.

    ```sql
    SELECT
        `exam_student`.`student_id`,
        `exams`.`course_id`,
        COUNT(*) AS `exam_attempts`,
        MAX(`exam_student`.`vote`) AS `max_vote`
    FROM `exam_student`
    JOIN `students` ON `exam_student`.`student_id` = `students`.`id`
    JOIN `exams` ON `exam_student`.`exam_id` = `exams`.`id`
    JOIN `courses` ON `exams`.`course_id` = `courses`.`id`
    GROUP BY `exam_student`.`student_id`, `exams`.`course_id`
    HAVING `max_vote` >= 18;
    ```