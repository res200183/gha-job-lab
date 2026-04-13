# GitHub Actions Job Lab

## Суть
Каждый job выполняется на новой машине  
Данные между jobs не передаются автоматически  

## Проблема
Job build создает файл test.txt  
Job test не видит этот файл  

Ошибка  
cat test.txt No such file or directory  

## Решение
Использовать artifacts  

## Пример

```yaml
name Job Test

on push

jobs
  build
    runs-on ubuntu-latest
    steps
      - run echo Hello from build job > test.txt
      - uses actions upload-artifact@v4
        with
          name build-file
          path test.txt

  test
    runs-on ubuntu-latest
    needs build
    steps
      - uses actions download-artifact@v4
        with
          name build-file
      - run cat test.txt
