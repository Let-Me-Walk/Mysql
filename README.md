# Mysql




21.列出薪金高于公司平均薪金的所有员工, 所在部门, 上级领导, 雇员的工资等级.


 1.select avg(sal) as avgsal from emp group by ename 
 2.select *from emp where sal>(select avg(sal) as avgsal from emp group by ename);
 3.select 
   d.dname,t.* 
  from 
   (select *from emp where sal>(select avg(sal) as avgsal from emp group by ename)) t 
  join 
   dept d
  on 
  t.deptno=d.deptno;
 4.select a.*,s.grade,e.ename as ld from (select 
   d.dname,t.* 
  from 
   (select *from emp where sal>(select avg(sal) as avgsal from emp group by ename)) t 
  join 
   dept d
  on 
  t.deptno=d.deptno) a join emp e on a.mgr=b.empno 
   join salgrade s
   on a.sal between s.losal and s.hisal;

  5.select e.ename as yg,l.ename as ld,d.dname,s.grade 
    from emp join dept d 
    on e.deptno=d.deptno
    join emp l
    on e.mgr=l.empno
    join salgrade s
    on e.sal between s.losal and s.hisal
    where e.sal >(select avg(sal) from emp);




22.列出与"SCOTT" 从事相同工作的所有员工及部门名称

+-------+--------+-----------+------+------------+---------+---------+--------+
| EMPNO | ENAME  | JOB       | MGR  | HIREDATE   | SAL     | COMM    | DEPTNO |
+-------+--------+-----------+------+------------+---------+---------+--------+
|  7369 | SMITH  | CLERK     | 7902 | 1980-12-17 |  800.00 |    NULL |     20 |
|  7499 | ALLEN  | SALESMAN  | 7698 | 1981-02-20 | 1600.00 |  300.00 |     30 |
|  7521 | WARD   | SALESMAN  | 7698 | 1981-02-22 | 1250.00 |  500.00 |     30 |
|  7566 | JONES  | MANAGER   | 7839 | 1981-04-02 | 2975.00 |    NULL |     20 |
|  7654 | MARTIN | SALESMAN  | 7698 | 1981-09-28 | 1250.00 | 1400.00 |     30 |
|  7698 | BLAKE  | MANAGER   | 7839 | 1981-05-01 | 2850.00 |    NULL |     30 |
|  7782 | CLARK  | MANAGER   | 7839 | 1981-06-09 | 2450.00 |    NULL |     10 |
|  7788 | SCOTT  | ANALYST   | 7566 | 1987-04-19 | 3000.00 |    NULL |     20 |
|  7839 | KING   | PRESIDENT | NULL | 1981-11-17 | 5000.00 |    NULL |     10 |
|  7844 | TURNER | SALESMAN  | 7698 | 1981-09-08 | 1500.00 |    0.00 |     30 |
|  7876 | ADAMS  | CLERK     | 7788 | 1987-05-23 | 1100.00 |    NULL |     20 |
|  7900 | JAMES  | CLERK     | 7698 | 1981-12-03 |  950.00 |    NULL |     30 |
|  7902 | FORD   | ANALYST   | 7566 | 1981-12-03 | 3000.00 |    NULL |     20 |
|  7934 | MILLER | CLERK     | 7782 | 1982-01-23 | 1300.00 |    NULL |     10 |
+-------+--------+-----------+------+------------+---------+---------+--------+
   1.select job from emp where ename='scott';
   2.select e.*,d.dname 
     from emp e
     join dept d
     on e.deptno=d.deptno
     having job in (select job from emp where ename='scott') and e.ename is not 'scott';

 23.列出薪金等于部门 30 中员工的薪金的其他员工的姓名和薪金.
   1.select ename,sal from emp where deptno=30;
   2.select t.*,e.ename,e.sal from emp e join ( select ename,sal from emp where deptno=30) t on e.sal =t.sal where deptno<>30;
   3.select distinct sal from emp where deptno =30;
   4.select ename,sal from emp where sal in (select distinct sal from emp where deptno =30) and deptno<>30;

 24.列出薪金高于在部门 30 工作的所有员工的薪金的员工姓名和薪金. 部门名称
   1.select max(sal) from emp where deptno=30;
   2.select e.*,d.dname from emp e join dept d on e.deptno=d.deptno where sal >(select max(sal) from emp where deptno=30);
   

 25.列出在每个部门工作的员工数量, 平均工资和平均服务期限

    没有员工的部门，部门人数是0
    1.select count(*) from emp group by deptno
    2.select avg(sal) from emp group by deptno
    3...

 
 26.列出所有员工的姓名、部门名称和工资
   select e.ename,e.sal,d.dname from emp e join dept d on e.deptno=d.deptno;


 27.列出所有部门的详细信息和人数
    select d.*,count(e.ename) from emp e join dept d on e.deptno group d.*;

 28. 列出各种工作的最低工资及从事此工作的雇员姓名
   select job,min(sal) as avgsal from emp group by job;
  select e.ename,t.avgsal  from emp e join ( select job,min(sal) as avgsal from emp group by job) t on e.job=t.job and e.sal = t.minsal;

 29.列出各个部门的 MANAGER( 领导) 的最低薪金
   select job,min(sal) from emp where job='manager' group by deptno;
  
