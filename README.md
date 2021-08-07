# JVMworking
## Понимание работы JVM

    public class JvmComprehension {

        public static void main(String[] args) {
            int i = 1;                      // 1
            Object o = new Object();        // 2
            Integer ii = 2;                 // 3
            printAll(o, i, ii);             // 4
            System.out.println("finished"); // 7
        }

        private static void printAll(Object o, int i, Integer ii) {
            Integer uselessVar = 700;                   // 5
            System.out.println(o.toString() + i + ii);  // 6
        }
    }
    
---
из файла JvmComprehension.java создается файл JvmComprehension.class
содержащий специальный байт-код для работы JVM

JvmComprehension.class начинает выполняться в JVM

JVM загружает классы JvmComprehension, Object, Integer, System
в Metaspace (имя, методы, поля) используя загрузчик Classloader 

в stack загружается фрейм main()

//1 в фрейме main() инициируется примитив i=1

//2 в куче выделяется место для обекта Object, в фрейме main() появляется ссылка на него - o

//3 в куче выделяется место для объекта Integer, в фрейме main() появляется ссылка на него - ii

//4 в stack загружается фрейм printAll(), в который передаются ссылки на o, i, ii

//5 в куче выделяется место для объекта Integer, в фрейме printAll() появляется ссылка на него - uselessVar

//6 в stack загружается фрейм (printAll)System.out.println, создается в куче новый String(o.toString() + i + ii) и создается на него ссылка в этом фрейме

//7

Из стека удаляются фреймы (printAll)System.out.println и printAll

Сборщик мусора (во время StopTheWorld) удаляет из кучи обекты uselessVar и String(o.toString() + i + ii) (на них больше нет активных ссылок)

в stack Создается новый фрейм System.out.println("finished"), в куче новый String, в фрейме ссылка на этот объект String
