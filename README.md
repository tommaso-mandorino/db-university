# Esercizio "Database università"

## Traccia

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