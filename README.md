# BD-barcelos
![image](https://user-images.githubusercontent.com/82169520/224718953-e21fd666-c7fc-456e-95d6-c165684cb531.png)

## Subconsultas

![image](https://user-images.githubusercontent.com/82169520/224714473-ddada970-fa72-420d-86de-df7db8c96d8f.png)
É possível inserir a subconsulta em várias cláusulas SQL, inclusive nestas:
Cláusula WHERE
Cláusula HAVING
Cláusula FROM
Na sintaxe:
operator inclui uma condição de comparação, como >, = ou
IN
Observação: As condições de comparação estão incluídas em
duas classes: operadores de uma única linha (>, =, >=, <, <>, <=) e de
várias linhas (IN, ANY, ALL)

![image](https://user-images.githubusercontent.com/82169520/224715414-20d5a836-580a-4c5a-baac-d1b72970fefe.png)

### Operador Having 
![image](https://user-images.githubusercontent.com/82169520/224715949-728deb66-6b10-40f1-be42-15364a470357.png)

A Cláusula HAVING com Subconsultas
É possível usar subconsultas tanto na cláusula WHERE como na cláusula HAVING. O servidor Oracle executa a subconsulta, e os resultados são retornados para a cláusula HAVING da consulta principal.

![image](https://user-images.githubusercontent.com/82169520/224716277-aeef95f2-5f2b-482e-b11d-49de65f8d5ed.png)

### Operador In

IN é um operador para especificar vários valores em uma cláusula WHERE. Com ele podemos verificar se determinada coluna está sendo mencionada em um determinado grupo de valores, seja ele definido manualmente ou através de subquerys.

SELECT last_name, salary, department_id
FROM employees
WHERE salary IN (2500, 4200, 4400, 6000, 7000, 8300,
8600, 17000);

### Operador Any

O operador ANY (e seu sinônimo, o operador SOME) compara um valor a cada valor retornado por uma subconsulta.
![image](https://user-images.githubusercontent.com/82169520/224717070-01c5c204-c4d4-4e6c-932a-0845c43099ac.png)

### Operador All

O operador ALL compara um valor com todos os valores retornados por uma subconsulta. 
![image](https://user-images.githubusercontent.com/82169520/224717444-ac923087-230a-4fc0-ab2a-e6acb8c4fda4.png)

### Valores Nulos em uma Subconsulta
![image](https://user-images.githubusercontent.com/82169520/224717966-457fa63e-8e95-48b2-a14c-0fe9bfdda3fc.png)

A instrução SQL do slide tenta exibir todos os funcionáriossem subordinados. Logicamente, essa instrução deveria ter retornado 12 linhas. No entanto, ela não retorna nenhuma linha. Um dos valores retornados pela consulta interna é um valor nulo e, por isso, a consulta inteira não retorna nenhuma linha. O motivo é que todas as condições que comparam um valor nulo resultam em um valor nulo. Dessa forma, sempre que houver a possibilidade de valores nulos integrarem o conjunto de resultados de uma subconsulta, não use o operador NOT IN. Esse operador corresponde a <> ALL. Observe que o valor nulo como parte do conjunto de resultados de uma subconsulta não representa um problema se você usa o operador IN. Esse perador corresponde a =ANY. Por exemplo, para exibir os funcionários com ubordinados, use a seguinte instrução SQL:SELECT emp.last_name
FROM employees emp
WHERE emp.employee_id IN
(SELECT mgr.manager_id
FROM employees mgr);

Como alternativa, é possível incluir uma cláusula WHERE na subconsulta para exibir
todos os funcionários sem subordinados:
SELECT last_name FROM employees
WHERE employee_id NOT IN
(SELECT manager_id
FROM employees
WHERE manager_id IS NOT NULL);

#### Exercicio 01 
O departamento de recursos humanos precisa de uma consulta que solicite ao usuário o sobrenome de um funcionário. A consulta exibe o sobrenome e a data de admissão de todos os funcionários no mesmo departamento do funcionário cujo nome foi fornecido (excluindo esse funcionário). Por exemplo, se o usuário informar Zlotkey, serão exibidos todos os funcionários que trabalham com Zlotkey (excluindo ele próprio).
&& - Duas vezes pede e guarda
& - pede o input do user (a janelinha)

SELECT last_name, hire_date
FROM   employees
WHERE  department_id = (SELECT department_id
                        FROM   employees
                        WHERE  last_name = '&&Enter_name')
AND    last_name <> '&Enter_name'; 

ou 
//Janelinha de input
select last_name, hire_date
from employees
where department_id in (
                select department_id
                from employees
                where last_name = initcap('&&v_ei'))
and last_name <> '&v_ei';

### Exercicio 02
Crie um relatório que exiba o número e o sobrenome de todos os funcionários cujo salário é maior que o salário médio. Classifique os resultados em ordem crescente de salário.

SELECT employee_id, last_name, salary
FROM   employees
WHERE  salary > (SELECT AVG(salary)
                 FROM   employees)
ORDER BY salary; 


### Exercicio 03
Crie uma consulta que exiba o número e o sobrenome de todos os funcionários que trabalham em um departamento com funcionários cujos sobrenomes
contêm a letra u. 

SELECT employee_id, last_name
FROM   employees
WHERE  department_id IN (SELECT department_id
                         FROM   employees
                         WHERE  last_name like '%u%');

### Exercicio 07
Modifique a consulta em lab_06_03.sql para exibir o número, o sobrenome,bem como o salário de todos os funcionários que ganham mais que o salário médio e trabalham em um departamento com funcionários cujos sobrenomes contêm a letra u.

SELECT employee_id, last_name, salary
FROM   employees
WHERE  department_id IN (SELECT department_id
                         FROM   employees
                         WHERE  last_name like '%u%')
AND    salary > (SELECT AVG(salary)	
                 FROM   employees);

## Conjuntos
