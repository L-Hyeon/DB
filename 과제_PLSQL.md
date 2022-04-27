```
DECLARE
    xDeptName varchar(20);
    xID varchar(5);
    xName varchar(20);
    xCourseID varchar(8);
    xSemester varchar(8);
    xYear int;
    xGrade varchar(2);
    xTitle varchar(30);
    xCredits int;
    xTotCred int;
    xSumGrade numeric(4, 1);

    CURSOR c1 is
        SELECT dept_name
        FROM department
        ORDER BY dept_name;

    CURSOR c2 is
        SELECT ID, name
        FROM student
        WHERE dept_name = xDeptName
        ORDER BY name;

    CURSOR c3 is
        SELECT course_id, semester, year, grade
        FROM takes
        WHERE ID = xID
        ORDER BY YEAR, semester DESC;

    CURSOR c4 is
        SELECT title, credits
        FROM course
        WHERE course_id = xCourseID;

BEGIN
    OPEN c1;
    LOOP
        FETCH c1 INTO xDeptName;
        EXIT WHEN c1%NOTFOUND;
        dbms_output.put_line (xDeptName);

        open c2;
        loop
            fetch c2 into xID, xName;
            exit when c2%NOTFOUND;
            dbms_output.put_line ('  '||xName);

            xTotCred := 0;
            xSumGrade := 0000.0;
            open c3;
            loop
                fetch c3 into xCourseID, xSemester, xYear, xGrade;
                exit when c3%NOTFOUND;

                open c4;
                fetch c4 into xTitle, xCredits;
                close c4;

                xTotCred := xTotCred + xCredits;
                if xGrade = 'A+' then
                xSumGrade := xSumGrade + 4.3*xCredits;
                elsif xGrade = 'A' then
                xSumGrade := xSumGrade + 4*xCredits;
                elsif xGrade = 'A-' then
                xSumGrade := xSumGrade + 3.7*xCredits;
                elsif xGrade = 'B+' then
                xSumGrade := xSumGrade + 3.3*xCredits;
                elsif xGrade = 'B' then
                xSumGrade := xSumGrade + 3*xCredits;
                elsif xGrade = 'B-' then
                xSumGrade := xSumGrade + 2.7*xCredits;
                elsif xGrade = 'C+' then
                xSumGrade := xSumGrade + 2.3*xCredits;
                elsif xGrade = 'C' then
                xSumGrade := xSumGrade + 2*xCredits;
                elsif xGrade = 'C-' then
                xSumGrade := xSumGrade + 1.7*xCredits;
                elsif xGrade = 'F' then
                xSumGrade := xSumGrade + 0;
                else
                xTotCred := xTotCred - xCredits;
                end if;

                if xGrade is null then
                    xGrade := '-';
                end if;

                dbms_output.put_line ('    '||xTitle||'  '||xCredits||'  '||xSemester||'  '||xYear||'  '||xGrade);

            end loop;
            close c3;

            if xTotCred = 0 then
                dbms_output.put_line ('    Total Credits: '||xTotCred||'  GPA: '||xSumGrade/1);
            else
                dbms_output.put_line ('    Total Credits: '||xTotCred||'  GPA: '||ROUND(xSumGrade/xTotCred, 1));
            end if;

        end loop;
        close c2;

    END LOOP;
    CLOSE c1;
END;
```

```
Statement processed.
Biology
  Tanaka
    Intro. to Biology  4  Summer  2017  A
    Genetics  4  Summer  2018  -
    Total Credits: 4  GPA: 4
Comp. Sci.
  Brown
    Intro. to Computer Science  4  Fall  2017  A
    Image Processing  3  Spring  2018  A
    Total Credits: 7  GPA: 4
  Shankar
    Game Design  4  Spring  2017  A
    Database System Concepts  3  Fall  2017  A
    Intro. to Computer Science  4  Fall  2017  C
    Robotics  3  Spring  2018  A
    Total Credits: 14  GPA: 3.4
  Williams
    Game Design  4  Spring  2017  B+
    Intro. to Computer Science  4  Fall  2017  A-
    Total Credits: 8  GPA: 3.5
  Zhang
    Database System Concepts  3  Fall  2017  A-
    Intro. to Computer Science  4  Fall  2017  A
    Total Credits: 7  GPA: 3.9
Elec. Eng.
  Aoi
    Intro. to Digital Systems  3  Spring  2017  C
    Total Credits: 3  GPA: 2
  Bourikas
    Intro. to Computer Science  4  Fall  2017  C-
    Robotics  3  Spring  2018  B
    Total Credits: 7  GPA: 2.3
Finance
  Chavez
    Investment Banking  3  Spring  2018  C+
    Total Credits: 3  GPA: 2.3
History
  Brandt
    World History  3  Spring  2018  B
    Total Credits: 3  GPA: 3
Music
  Sanchez
    Music Video Production  3  Spring  2018  A-
    Total Credits: 3  GPA: 3.7
Physics
  Levy
    Intro. to Computer Science  4  Fall  2017  F
    Image Processing  3  Spring  2018  B
    Intro. to Computer Science  4  Spring  2018  B+
    Total Credits: 11  GPA: 2
  Peltier
    Physical Principles  4  Fall  2017  B-
    Total Credits: 4  GPA: 2.7
  Snow
    Total Credits: 0  GPA: 0
```

<img src="https://github.com/L-Hyun/L-Hyun.github.io/blob/main/assets/DB/tplsql-1.png?raw=true" />
