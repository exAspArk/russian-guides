# Ruby style гайд

* Исходный код
* Синтаксис

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

* Используйте наиболее короткие формы, если они в вашем случае идентичны, однако, все же по возможности старайтесь использовать `if`:

    ```Ruby
    # плохо
    do_something unless object.nil?

    # хорошо
    do_something if object
    
    # плохо
    do_something if object.nil?

    # хорошо
    do_something unless object
    
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
    
* Не ставьте скобки вокруг параметров внутренних DSL-методов (Rake, Rails, RSpec,...); методов, являющимися ключевыми словами в Ruby (`attr_reader`, `puts`,...), и методов без скобок (например, геттеров).

    ```Ruby
    # хорошо
    class Person
      attr_reader :name, :age
      # omitted
    end

    temperance = Person.new('Temperance', 30)
    temperance.name

    puts temperance.age

    x = Math.sin(y)
    array.delete(e)
    ```

* Для однострочных блоков используйте `{...}` вместо `do...end`. Используйте `do...end` для многострочных блоков. Не используйте `do...end` при чейнинге (chaining) - цепочке вызовов блоков.

    ```Ruby
    names = ['Bozhidar', 'Steve', 'Sarah']

    # хорошо
    names.each { |name| puts name }

    # плохо
    names.each do |name|
      puts name
    end

    # хорошо
    names.select { |name| name.start_with?('S') }.map { |name| name.upcase }

    # плохо
    names.select do |name|
      name.start_with?('S')
    end.map { |name| name.upcase }
    ```

    Однако, когда вы чейните несколько блоков с использованием `{...}` вы должны задуматься, действительно ли код хорошо читаем, и можно ли разделить как-то упростить код?

* Не используйте `return`, если он не обязателен.

    ```Ruby
    # плохо
    def some_method(some_arr)
      return some_arr.size
    end

    # хорошо
    def some_method(some_arr)
      some_arr.size
    end
    
    # нормально
    def some_method(some_arr)
      some_arr.each do |elem|
        return elem if elem
      end
    end
    ```

* Используйте `self` только там, где он необходим (только при изменении свойства).

    ```Ruby
    # плохо
    def ready?
      if self.last_reviewed_at > self.last_updated_at
        self.worker.update(self.content, self.options)
        self.status = :in_progress
      end
      self.status == :verified
    end

    # хорошо
    def ready?
      if last_reviewed_at > last_updated_at
        worker.update(content, options)
        self.status = :in_progress
      end
      status == :verified
    end
    ```

* В методах не перекрывайте входящими параметрами свойства класса.

    ```Ruby
    class Foo
      attr_accessor :options

      # нормально
      def initialize(options)
        # options и self.options именно здесь равны друг другу
        self.options = options
      end

      # плохо
      def do_something(options = {})
        unless options[:when] == :later
          # приходится писать `self`, так как локальная переменная перекрыла свойство класса
          output(self.options[:message])
        end
      end

      # хорошо
      def do_something(params = {})
        unless params[:when] == :later
          output(options[:message])
        end
      end
    end
    ```

* Ставьте пробелы вокруг `=`, когда присваиваете значения по умолчанию параметрам метода:

    ```Ruby
    # плохо
    def some_method(arg1=:default, arg2=nil, arg3=[])
      # do something...
    end

    # хорошо
    def some_method(arg1 = :default, arg2 = nil, arg3 = [])
      # do something...
    end
    ```

* Не используйте перевод строки для продолжения кода (\\).

    ```Ruby
    # плохо
    result = 1 - \
             2

    # все равно выглядит очень и очень плохо
    result = 1 \
             - 2
    ```

* Не используйте `||=` для объявления булевых переменных. Напомню, что значение присваивается переменной, если до этого оно имело значение `nil` или `false`.

    ```Ruby
    # плохо
    enabled ||= true

    # хорошо
    enabled = true if enabled.nil?
    ```

* Старайтесь не использовать Perl-style объявления переменных (например: `$0-9`, `$`,...).

* Никогда не вставляйте пробел между именем метода и открывающейся скобкой.

    ```Ruby
    # плохо
    f (3 + 2) + 1

    # хорошо
    f(3 + 2) + 1
    ```

* Используйте новый синтаксис лямбды.

    ```Ruby
    # плохо
    lambda = lambda { |a, b| a + b }
    lambda.call(1, 2)

    # хорошо
    lambda = ->(a, b) { a + b }
    lambda.(1, 2)
    ```

* Используйте `_` для неиспользуемых параметров блока.

    ```Ruby
    # плохо
    result = hash.map { |k, v| v + 1 }

    # хорошо
    result = hash.map { |_, v| v + 1 }
    ```

## Нейминг

> Я думаю, все хорошие программисты согласятся, что иногда придумать название - самая сложная задача :)

* Называйте переменные на английском языке, не занимайтесь словообразованиями с использованием транслита. Вот пример, с которым приходилось лично сталкиваться:

    ```Ruby
    # плОхО
    balls = 1000

    # хорошо
    points = 1000
    ```

* Используйте `snake_case` для методов и переменных.

* Используйте `CamelCase` для классов и модулей. Аббревиатуры (например, HTTP, RFC, XML) можно так и именовать полностью в верхнем регистре.

* Используйте `SCREAMING_SNAKE_CASE` для констант.

* Названия методов, возращающих булево значение, должно заканчиваться вопросительным знаком. Например, `Array#empty?`.

* Названия потенциально "опасных" методов (например тех, которые изменяют `self` or аргументы, `exit!`, который не запускает финализацию, как это делает `exit` и т.д.) должны заканчиваться восклицательным знаком, если есть "безопасная" версия этого метода.

    ```Ruby
    # плохо - нет 'безопасного' метода
    class Person
      def update!
      end
    end

    # теперь хорошо
    class Person
      def update
      end
    end

    # хорошо
    class Person
      def update!
      end

      def update
      end
    end
    ```

* Допускается использовать метод `reduce` с коротким блоком, используя названия аргуметов `|a, e|` от accumulator и element, `each` - `|k, v|` от key и value, `each_with_index` - `|e, i|` от element и index,...

* Определяя бинарные операторы, используйте `other` в качестве названия аргумента.

    ```Ruby
    # хорошо
    def +(other)
      # body omitted
    end
    ```

* Используйте `map` вместо `collect`, `find` вместо `detect`, `select` вместо
  `find_all`, `reduce` вместо `inject` и `size` вместо `length`. Будет лучше, если все будут придерживаться этих методов.

* Используйте `flat_map` вместо `map` + `flatten` для массивов с глубиной не более 2.

    ```Ruby
    # плохо
    all_songs = users.map(&:songs).flatten.uniq

    # хорошо
    all_songs = users.flat_map(&:songs).uniq
    ```

## Комментарии

> Хороший код не требуется в документировании.

* Пишите код, который не нуждается в излишнем комментировании, и этот раздел можно будет пропустить.

* Пишите комментарии на английском, английскими словами.

* После знака `#` ставится один пробел.

* Комментарии длиной в одно слово пишутся с заглавной буквы с использованием знаков препинания, если необходимо.
  
* Не будьте капитаном очевидностью.

    ```Ruby
    # плохо
    counter += 1 # increments counter by one
    ```

* Поддерживайте существующие комментарии (удаляйте или обновляйте их), так как присутствие некорректных комментариев хуже, чем их отсутствие.

* Вместо того, чтобы писать комментарии к плохому коду, лучше отрефакторьте его.
