# Ruby style гайд

* [Исходный код]()
* [Синтаксис]()

## Исходный код

* Сохраняйте исходные файлы в кодировке `UTF-8`.
* Используйте 2 **пробела** в качестве отступа, не использовать табы.

    ```Ruby
    # хорошо - 2 пробела
    def some_method
      do_something
    end

    # плохо - 4 пробела
    def some_method
        do_something
    end
    ```

* Используйте в качестве окончаний строк (line endings) Unix-style (в *BSD/Solaris/Linux/OSX по умолчанию). Пользователям Windows стоит быть более аккуратными.

* Ставьте пробелы вокруг операторов; после запятых, двоеточий и точек с запятыми; вокруг `{` и после `}`. Все это позволяет сделать код более читаемым.

    ```Ruby
    # хорошо
    sum = 1 + 2
    a, b = 1, 2
    1 > 2 ? true : false; puts 'Hi'
    [1, 2, 3].each { |e| puts e }
    ```

    Исключением является оператор возведения в степень:

    ```Ruby
    # плохо
    e = M * c ** 2

    # хорошо
    e = M * c**2
    ```

* Не ставьте пробелы после символо `(`, `[` и до `]`, `)`.

    ```Ruby
    # хорошо
    some(arg).other
    [1, 2, 3].length
    ```

* Отступ конструкции `when` должен быть на том же уровне, что и `case`. Уверен, обязательно найдутся те, кто будет не согласен, но так принято в мире ruby :)

    ```Ruby
    # хорошо
    case
    when song.name == 'Misty'
      puts 'Not again!'
    when song.duration > 120
      puts 'Too long!'
    when Time.now.hour > 21
      puts "It's too late"
    else
      song.play
    end

    kind = case year
           when 1850..1889 then 'Blues'
           when 1890..1909 then 'Ragtime'
           when 1910..1929 then 'New Orleans Jazz'
           when 1930..1939 then 'Swing'
           when 1940..1950 then 'Bebop'
           else 'Jazz'
           end
    ```

* Добавляйте пустую строку между `def`ами, разбивая методы на логические блоки.

    ```Ruby
    # хорошо
    def some_method
      data = initialize(options)
      data.manipulate!
      data.result
    end

    def some_method
      result
    end
    ```

* Длина строк ориентировочно не должна превышать 110 символов (ИМХО).
* Выравнивайте параметры, передаваемые в метод, если они не помещаются на одной строке (ИМХО).

    ```Ruby
    # плохо
    def send_mail(source)
      Mailer.deliver(to: 'bob@example.com', from: 'us@example.com', subject: 'Important message', body: source.text)
    end

    # все равно плохо
    def send_mail(source)
      Mailer.deliver(
        to: 'bob@example.com',
        from: 'us@example.com',
        subject: 'Important message',
        body: source.text)
    end

    # плохо (2 отступа)
    def send_mail(source)
      Mailer.deliver(
          to: 'bob@example.com',
          from: 'us@example.com',
          subject: 'Important message',
          body: source.text)
    end

    # хорошо
    def send_mail(source)
      Mailer.deliver(to: 'bob@example.com',
                     from: 'us@example.com',
                     subject: 'Important message',
                     body: source.text)
    end
    ```

* Используйте нижние подчеркивания для улучшения читаемости больших чисел.

    ```Ruby
    # плохо - черт, сколько тут нулей??
    num = 1000000000

    # хорошо - жить становится проще
    num = 1_000_000_000
    ```

* Добавляя комментарий к методу (RDoc), не добавляйте пустую строку перед `def`.
* Избегайте пробелы в конце строк (trailing whitespace).

## Синтаксис

* Ставьте скобки, если в метод `def` передаются аргументы.

     ```Ruby
     # хорошо
     def some_method
       # body
     end
     
     # это поможет тем бедным людям, у которых нет подсветки синтаксиса
     def some_method_with_argument(arg)
       # body
     end

     def some_method_with_arguments(arg1, arg2)
       # body
     end
     ```

* Не используйте `for`, если вы абсюлютно не уверены в том, что он вам действительно нужен. В большинстве случаев вам нужен `each`. Проблема в том, что `for` не создает новый блок, из-за чего объявленные переменные видны снаружи.

    ```Ruby
    arr = [1, 2, 3]

    # плохо
    for elem in arr do
      puts elem
    end

    # хорошо
    arr.each { |elem| puts elem }
    ```

* Никогда не используйте `then` для многострочных `if/unless`.

    ```Ruby
    # плохо
    if some_condition then
      # body omitted
    end

    # хорошо
    if some_condition
      # body omitted
    end
    ```

* Используйте тернарный оператор `?:` вместо однострочной конструкции `if/then/else/end`. Так код выглядит более очевидным и кратким.

    ```Ruby
    # плохо
    result = if some_condition then something else something_else end

    # хорошо
    result = some_condition ? something : something_else
    ```

* Но не стоит злоупотреблять, используйте только один тернарный оператор в одной строке. Да и вообще, если строка становится слишком сложной (более 3х слов) или длинной. В этих случаях используйте конструкцию `if/else`.

    ```Ruby
    # плохо
    some_condition ? (nested_condition ? nested_something : nested_something_else) : something_else
    
    # это тоже плохо
    some_object.some_method? ? something(args).first : something_else(args)
    
    # хорошо
    if some_condition
      nested_condition ? nested_something : nested_something_else
    else
      something_else
    end
    ```

* Если вы на машине времени прибыли к нам из прошлого, не используйте `if condition: ...` - это было выпилино, начиная с Ruby 1.9. То же и относится к `when x: ...`.

    ```Ruby
    # плохо
    result = if some_condition: something else something_else end

    # хорошо
    result = some_condition ? something : something_else
    ```

* Используйте `&&/||` для логических выражений, `and/or` для управления потоком (control flow). Есть такое правило: если вы используете скобки, вы используете не те операторы (то есть они имеют разные приоритеты). Надеюсь, по примеру станет понятно, и вы никогда не будете их путать.

    ```Ruby
    # логические выражения
    if some_condition && some_other_condition
      do_something
    end

    # управление потоком
    document.saved? or document.save!
    ```

* Используйте модификацию `if/unless`, если код простой и помещается в одной строке.

    ```Ruby
    # плохо
    if some_condition
      do_something
    end

    # хорошо
    do_something if some_condition
    ```

* Используйте `unless` вместо `if` для отрицательных условий, например:

    ```Ruby
    # плохо
    do_something if !some_condition

    # хорошо
    do_something unless some_condition
    ```

* Однако все же по возможности старайтесь использовать `if`:

    ```Ruby
    # плохо
    do_something unless object.nil?

    # хорошо
    do_something if object
    
    # не так хорошо
    do_something unless array.empty?
    
    # хорошо
    do_something if array.any?
    ```

* Никогда не используйте `unless` в связке с `else`. Перепишите конструкцию, поменяв плоки местами.

    ```Ruby
    # плохо
    unless success?
      puts 'failure'
    else
      puts 'success'
    end

    # хорошо
    if success?
      puts 'success'
    else
      puts 'failure'
    end
    ```

* Не используйте скобки вокруг условий для `if/unless/while`.

    ```Ruby
    # плохо
    if (x > 10)
      # body omitted
    end

    # хорошо
    if x > 10
      # body omitted
    end
    ```

* Используйте модификацию `while/until`, если код можно поместить в одной простой строке.

    ```Ruby
    # плохо
    while some_condition
      do_something
    end

    # хорошо
    do_something while some_condition
    ```