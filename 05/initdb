#! /usr/bin/env python3
# -*- coding:UTF-8 -*-

from dbconn import db_cursor

def create_db():
    sqlstr = """
    -- 删除旧表
    DROP TABLE IF EXISTS course_grade;
    DROP TABLE IF EXISTS student;
    DROP TABLE IF EXISTS course;

    -- === 学生表
    CREATE TABLE IF NOT EXISTS student  (
        sn       INTEGER,     --序号
        no       VARCHAR(10), --学号
        name     TEXT,        --姓名
        gender   CHAR(1),     --性别(F/M/O)
        enrolled DATE,        --入学时间
        PRIMARY KEY(sn)
    );

    -- 给sn创建一个自增序号
    CREATE SEQUENCE seq_student_sn 
        START 10000 INCREMENT 1 OWNED BY student.sn;
    ALTER TABLE student ALTER sn 
        SET DEFAULT nextval('seq_student_sn');
    -- 学号唯一
    CREATE UNIQUE INDEX idx_student_no ON student(no);

    -- === 课程表
    
    CREATE TABLE IF NOT EXISTS course  (
        sn       INTEGER,     --序号
        no       VARCHAR(10), --课程号
        name     TEXT,        --课程名称
        PRIMARY KEY(sn)
    );
    CREATE SEQUENCE seq_course_sn 
        START 10000 INCREMENT 1 OWNED BY course.sn;
    ALTER TABLE course ALTER sn 
        SET DEFAULT nextval('seq_course_sn');
    CREATE UNIQUE INDEX idx_course_no ON course(no);


    -- === 成绩表
    CREATE TABLE IF NOT EXISTS course_grade  (
        stu_sn INTEGER,      -- 学生序号
        cou_sn INTEGER,      -- 课程序号
        grade  NUMERIC(5,2), -- 最终成绩
        PRIMARY KEY(stu_sn, cou_sn)
    );

    ALTER TABLE course_grade 
        ADD CONSTRAINT stu_sn_fk FOREIGN KEY (stu_sn)
        REFERENCES student(sn);
    ALTER TABLE course_grade 
        ADD CONSTRAINT cou_sn_fk FOREIGN KEY (cou_sn)
        REFERENCES course(sn);
    """
    with db_cursor() as cur :
        cur.execute(sqlstr) # 执行SQL语句
    
def init_data():
    sqlstr = """
    DELETE FROM course_grade;
    DELETE FROM student;
    DELETE FROM course;

    INSERT INTO student (sn, no, name,gender ,enrolled)  VALUES (101, 'S001',  '张三',  '男', '2013-09-08');
    INSERT INTO student (sn, no, name,gender,enrolled)  VALUES 
        (102, 'S002',  '李四',  '女', '2012-09-06'), 
        (103, 'S003',  '王五',  '女', '2014-09-05'),
        (104, 'S004',  '马六',  '男', '2011-08-25');

    INSERT INTO course (sn, no, name)  VALUES 
        (101, 'C01',  '高数'), 
        (102, 'C02',  '外语'),
        (103, 'C03',  '线代');

    INSERT INTO course_grade (stu_sn, cou_sn, grade)  VALUES 
        (101, 101,  91), 
        (102, 101,  89),
        (103, 101,  90),
        (101, 102,  89);
    """
    with db_cursor() as cur :
        cur.execute(sqlstr)    

if __name__ == '__main__':
    create_db()
    init_data()
    print('数据库已初始化完毕！')
    

