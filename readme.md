<p align="center"><b>МОНУ НТУУ КПІ ім. Ігоря Сікорського ФПМ СПіСКС</b></p>
<p align="center">
<b>Звіт з лабораторної роботи 3</b><br/>
"Конструктивний і деструктивний підходи до роботи зі списками"<br/>
дисципліни "Вступ до функціонального програмування"
</p>
<p align="right"><b>Студент</b>: Іващук Дмитро Сергійович</p>
<p align="right"><b>Рік</b>: 2024</p>

## Загальне завдання

Реалізуйте алгоритм сортування чисел у списку двома способами: функціонально і імперативно.
1. Функціональний варіант реалізації має базуватись на використанні рекурсії і конструюванні нових списків щоразу, коли необхідно виконати зміну вхідного списку. Не допускається використання: деструктивних операцій, циклів, функцій вищого порядку або функцій для роботи зі списками/послідовностями, що використовуються як функції вищого порядку. Також реалізована функція не має бути функціоналом (тобто приймати на вхід функції в якості аргументів).
2. Імперативний варіант реалізації має базуватись на використанні циклів і деструктивних функцій (псевдофункцій). Не допускається використання функцій вищого порядку або функцій для роботи зі списками/послідовностями, що використовуються як функції вищого порядку. Тим не менш, оригінальний список цей варіант реалізації також не має змінювати, тому перед виконанням деструктивних змін варто застосувати функцію copy-list (в разі необхідності). Також реалізована функція не має бути функціоналом (тобто приймати на вхід функції в якості аргументів). 
Алгоритм, який необхідно реалізувати, задається варіантом (п. 3.1.1). Зміст і шаблон звіту наведені в п. 3.2. 
Кожна реалізована функція має бути протестована для різних тестових наборів. 
Тести мають бути оформленні у вигляді модульних тестів (наприклад, як наведено у п. 2.3).

## Варіант 6

Алгоритм сортування вставкою №1 (з лінійним пошуком зліва) за незменшенням.

## Лістинг функції з використанням конструктивного підходу

```lisp
(defun recursive-sort (lst)
  (labels ((insert (value sorted-list)
             (if (or (null sorted-list) (<= value (first sorted-list)))
                 (cons value sorted-list)
                 (cons (first sorted-list) (insert value (rest sorted-list))))))
    (if (null lst)
        nil
        (insert (first lst) (recursive-sort (rest lst))))))
```

### Тестові набори

```lisp
(defun check-recursive-sort-test (name input expected)
  (format t "~:[failed~;passed~] ~a~%"
    (equal (recusive-sort input) expected)
      name))

(defun test-recursive-sort ()
  (check-recursive-sort-test "test 1" '(5 3 8 1 2) '(1 2 3 5 8))
  (check-recursive-sort-test "test 2" '(8 7 6 5 4) '(4 5 6 7 8))
  (check-recursive-sort-test "test 3" '(1 1 9 1) '(1 1 1 9))
  (check-loop-sort-test "test 4" '(0) '(0)))
```

### Тестування
```lisp
CL-USER> (test-recursive-sort)
passed test 1
passed test 2
passed test 3
passed test 4
NIL
```

### Лістинг функції з використанням деструктивного підходу

```lisp
(defun loop-sort (list)
  (let ((sorted-list (copy-list list)))
    (loop for i from 1 below (length sorted-list) do
          (let ((current (nth i sorted-list))
                (j i))
            (loop while (and (> j 0)
                             (< current (nth (1- j) sorted-list))) do
                  (setf (nth j sorted-list) (nth (1- j) sorted-list))
                  (decf j))
            (setf (nth j sorted-list) current)))                      
    sorted-list))
```

### Тестові набори

```lisp
(defun check-loop-sort-test (name input expected)
  (format t "~:[failed~;passed~] ~a~%"
    (equal (loop-sort input) expected)
      name))

(defun test-loop-sort ()
  (check-loop-sort-test "test 1" '(5 3 8 1 2) '(1 2 3 5 8))
  (check-loop-sort-test "test 2" '(8 7 6 5 4) '(4 5 6 7 8))
  (check-loop-sort-test "test 3" '(1 1 9 1) '(1 1 1 9))
  (check-loop-sort-test "test 4" '(0) '(0)))
```

### Тестування
CL-USER> (test-loop-sort)
passed test 1
passed test 2
passed test 3
passed test 4
NIL
```
