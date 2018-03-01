# Models

### Create application

Create a new Rails application called **StudendsApp**
```
$ rails new StudensApp
```

<br/>

### Generate Model

Define model named **Student**

* **firstname**
   * string
* **lastname**
    * string
* **age**
  * integer
  * should be higher or equal 15
* **grade**
  * integer
  * reperesentation in database:
    * A = 4
    * B = 3
    * C = 2
    * D = 1
    * F = 0
* **gender**
  * integer
  * reperesentation in database:
    * male = 1
    * female = 2
* **playing_sport**
  * boolean

**References**
* http://guides.rubyonrails.org/v3.2/migrations.html (1.4 Supported Types)
* https://en.wikipedia.org/wiki/Academic_grading_in_the_United_States
* https://en.wikipedia.org/wiki/ISO/IEC_5218

**Generate model**
```
$ rails g model Student firstname lastname age:integer grade:integer gender:integer playing_sport:boolean
```

<br/>

### Run migrations

Update database by running migrations.

```
$ rake db:migrate
```

<br/>

### Open Rails Console

Open rails console

```bash
$ rails c
```

Anytime to clear the screen

```ruby
system "clear"
```

<br/>

### Have fun 

Create 1 student

```ruby
Student.create(firstname: "James", lastname: "Dean")
```

Get student by ID

```ruby
Student.find(1)
```

Get students with firstname "James"

```ruby
students = Student.where(firstname: "James")
students[0]
students[0].firstname
students.empty?
students.any?

# https://ruby-doc.org/core-2.2.0/Array.html
```

Delete student by ID

```ruby
Student.destroy(1)  # will run callbacks
Student.delete(1)   # will NOT run callbacks

# http://guides.rubyonrails.org/active_record_callbacks.html
```

Create 5 students

```ruby
Student.create(firstname: "Francisco", lastname: "Chang",   age: 25, grade: 2, gender: 1, playing_sport: false)
Student.create(firstname: "Roland",    lastname: "Mendel",  age: 22, grade: 4, gender: 1, playing_sport: true)
Student.create(firstname: "Helen",     lastname: "Bennett", age: 19, grade: 4, gender: 2, playing_sport: true)
Student.create(firstname: "Maria",     lastname: "Anders",  age: 18, grade: 3, gender: 2, playing_sport: false)
Student.create(firstname: "Giovanni",  lastname: "Rovelli", age: 20, grade: 0, gender: 1, playing_sport: true)
```

Get number of students

```ruby
Student.count   # => 5
```

Get average age, Get average grade

```ruby
"%.2f" % Student.average(:age)     # => 20.80
"%.2f" % Student.average(:grade)   # =>  2.60

# http://stackoverflow.com/questions/10261489/removing-scientific-notation-from-float
```

Get only student with age > 20

```ruby
Student.where("age > 20")
```

Print how many students are playing_sport

```ruby
Student.where(playing_sport: true).count   # => 3
```

Print how many many students are male, how many females

```ruby
Student.group(:gender).count   # => {1=>3, 2=>2}
```

<br/>

### Improve Student model

Delete all Students first

```ruby
Student.delete_all
```

Save Student with wrong data

```ruby
Student.create(firstname: "John", lastname: "Bad", age: -2, grade: 8, gender: 4, playing_sport: true)
```

Check that Student with wrong data was saved

```ruby
Student.last  # returns last saved Student
```

Update **student.rb** file

```ruby
class Student < ApplicationRecord
    validates :age, numericality: {
        greater_than_or_equal_to: 15
    }

    enum grade: {
        "A": 4,
        "B": 3,
        "C": 2,
        "D": 1,
        "F": 0
    }

    enum gender: {
        "male": 1,
        "female": 2
    } 
end

# http://guides.rubyonrails.org/active_record_validations.html
```

Reload Rails console to apply changes

```
reload!  # written inside rails console
```

Now we can save Student, with valid data only, and in a more fancy way
```ruby
# Will NOT save
# Age has to be bigger or equal 15
# Gender must be male or female (or 1 and 2 works too)
# Gender must be A, B, C, D, or F (or 4, 3, 2, 1, 0 works too)

Student.create(firstname: "John", lastname: "Bad", age: -2, grade: 8, gender: 4, playing_sport: true)


# Will save
# Now we can set grades as A, B, C, D, F, in database it gets saved as corresponding number
# Now we can set gender as male or female, in database it gets saved as corresponding number

Student.create(firstname: "John", lastname: "Good", age: 20, grade: "C", gender: "male", playing_sport: true)
```

We can use fancy names everywhere now
```ruby
Student.where(gender: "male")
Student.where(grade: ["C", "D"])  # students with grades C, or D
```

<br/>

### ~ END ~ :)


