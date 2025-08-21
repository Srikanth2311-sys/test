# test


DECLARE
    student_rec RECORD;
BEGIN
    -- Drop temp table if it already exists
    IF EXISTS (SELECT FROM pg_tables WHERE schemaname = 'pg_temp' AND tablename = 'temp_students') THEN
        EXECUTE 'DROP TABLE temp_students';
    END IF;

   

    -- Loop through each row
    FOR student_rec IN  SELECT * FROM school.students LOOP
        RAISE NOTICE 'Processing student: %, Age: %', student_rec.name, student_rec.age;

        INSERT INTO student_processing_log (student_name, age)
        VALUES (student_rec.name, student_rec.age);

        PERFORM pg_sleep(1);

        COMMIT;  -- Only works if CALL is outside a transaction block
    END LOOP;
END;
