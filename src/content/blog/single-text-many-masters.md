---
title: "Single text, many masters"
pubDate: 2011-05-21
blog: literate-programming
---


Hey everyone. Here’s a draft of an essay I’ve been working on. I’d love to hear your feedback.

The word ‘engineering’ has a deep connection to the word ‘trade-offs’ in my mind. Most engineering decisions come down to evaluating a few differing alternatives, and often multiple factors end up being negatively correlated. You can make something stronger, but then it will be heavier. It can be made faster, but then it’s significantly more expensive. A good engineer is able to take all of these factors into account, and design a system such that it maximizes its effectiveness across the sum of all of the relevant constraints. No matter if you consider the act of writing software an art, science, or engineering, its indisputable that designing complex software systems is identical in this respect. There are dozens of different metrics that system architects take into consideration while crafting a plan of attack, but but there’s a deeper question of balance here that’s significantly different than these more traditional engineering issues.

Text, in the form of source code, presents unique challenges of composition. These difficulties all stem from the same root: source code is a singular text, but must be intelligible to multiple, simultaneous audiences. More traditional forms of authorship still take audience into consideration, of course, but the decision to engage a diverse group of people is the choice of the writer. While mass appeal may be something that certain authors strive to attain, it’s not an inherent property of their chosen form of expression. Source code, while text, inhabits a multiplicity of forms, and software developers are confronted with this inherent multi-faceted complexity when composing any particular software work. Some of these forms suit certain audiences better than others, and so it falls to the programmer to manage which form they are currently working in, consider which audience they are attempting to write for, and arrange all of these forms amongst one another in such a way that any particular audience is able to navigate and extract the proper information from the source without confusion.

In this post, I’ll expand on the concept of audiences for code, and in a future post, I’ll explore the simultaneous forms that code takes.

## The multitude of audiences

### The default audience: the computer

> Science is what we understand well enough to explain to a computer. Art is everything else we do.Don Knuth
> 

This may seem obvious, but the when considering the question of “Who are programs written for?”, many would say “The computer. Duh.” In many ways, a computer is the primary reader of a particular piece of source code. The code that’s given to a computer will be executed billions of times per second, repeated, recalculated, and re-interpreted over and over and over again.

The computer’s native understanding of software comes from machine code. Machine code are the binary numbers that the CPU loads into memory and processes directly. For example, here’s a line of machine code for an x86 machine that puts the value ‘97’ into the AL register, graciously stolen [from Wikipedia](http://en.wikipedia.org/wiki/Assembly_language#Assembly_language):

```
10110000 01100001
```

While most would consider this unintelligible, computers were actually programmed this way at one time. My uncle actually did this by flipping switches to set the binary and pushed a button to store it in memory. Unfortunately, what’s good for the computer isn’t good for the human programmer. This is why assembly language was created. Assembly language has a 1 to 1 mapping to machine code, but is much easier for humans to understand. Here’s that same line in assembly:

```
MOV AL, 61h       ; Load AL with 97 decimal (61 hex)
```

The `MOV` corresponds with `10110`, `AL` maps to `000`, and 61 hex is `01100001`. `MOV` is short for ‘move,’ though, and this mnemonic is just a bit easier to understand than `10110`. This is the most basic example of a compositional trade-off. It’s not a true trade-off, because they map perfectly to one another, but it illustrates the difference between composing in a language that the computer understands and one that’s more natural for the programmer. Another important concept comes into play, that of *compilation*. Virtually every work of composition created in software is automatically translated to another form before it is executed. We’ll address this concept more fully when we discuss form later.

If that’s where assembly stopped, it would remain a 1 to 1 mapping. However, virtually every assembly language also offers macros, and this moves the code further away from the machine and destroys the synchrony of the two forms. Here’s an example:

```
MOV EAX, [EBX]
```

The `[]` characters change the meaning of `EBX`, rather than be the value stored in that particular register, they imply that the value is a memory address, and we want to move the contents of that address to `EAX`. The generated machine code could now be processed into multiple valid assembly forms, and so the transformation is only perfect in one direction, even if it’s possible to ‘decompile’ it into one of those possible encodings. This is considered to be an acceptable trade-off for human readability; we very rarely want to turn machine code back into assembly.

There’s also a jump between higher level languages, as well. Here’s the assembly statements that adds 2 and 3 together:

```
MOV EAX, 2
ADD EAX, 3
```

`ADD`, of course, is the statement that adds a number to the register that’s given. Now `EAX` has the value 5. Here’s the same code, but in C:

```
int x = 2;
x = x + 3;
```

Pretty simple. You can see how the C is much easier to understand; we say what type `x` is (an integer), and it’s a bit more explicit. `x` is equal to `x` + three. However, since the C is divorced from the machine, and is written for the person, we can change our compiler to make assembly code that works on a different kind of processor architecture. If we were to compile the above C for x86_64, a 64 bit version of x86, we might get some assembly that’d look like this:

```
MOVL RAX, 2
ADDL RAX, 3
```

While this code looks similar to the above, it is quite different. This uses the native 64 bit types, rather than the 32 bit types above. The other important thing is that by writing code that’s divorced from the machine, and written for people, we’re able to translate it into the code for multiple machines. If we had written the assembly above, when moving to another architecture, it would have required a total re-write. And while this particular sample looks very similar, a more complex piece of code will be significantly divergent, but I don’t want to go into the details of two kinds of assembly code. Because we can define the languages for humans, and the language of computers is somewhat beholden to the physical machine itself, it’s significantly easier to do the translation from C to the two kinds of machines, rather than trying to translate from one machine to another. What we lose in this kind of translation, though, is efficiency. Code that was hand-crafted for each machine would be more performant, and better represent each individual platform.

Even though we may choose to use a language that’s more understandable to people, it still has to be understood by the computer in some form. This translation will introduce some amount of penalty, and so it’s important that this gets taken into consideration. Sometimes, code must be written in a way that’s not easy for a person to read, because it’s easier for the computer to be efficient with a more opaque implementation.

### The reflexive audience: the programmer himself

> Debugging is twice as hard as writing the code in the first place. Therefore, if you write the code as cleverly as possible, you are, by definition, not smart enough to debug it.Brian Kernighan
> 

Everyone who writes code has experienced this at some time or another. You write a whole bunch of code, and then a few months goes by, and you take a look at it, and it’s absolutely unintelligible. This happens because at the time of inception, the author of a particular piece of code has an incredibly close relationship to it. As it was just written, the code is obvious to the author. They’re in the proper mindset to understand the intention that was drawn upon to necessitate bringing those lines into the world, and so no extra explanation is necessary. As time goes on, however, the author becomes more close to the third audience, other programmers. It’s important for coders to recognize this fact, and take preventative steps to ameliorate this confusion.

Even though the author will approach the position of the other audience eventually, this audience is distinct because there is a certain level of explanation that sits between undocumented, inexpressive code and code that’s well explained, and this is the position most code is in. An explanation that’s helpful to those who understand the code, but not to those who don’t is better than nothing. This sort of code may be overly contextual, and could use some added information to improve its clarity.

### The other audience: colleagues and coworkers

> Always code as if the guy who ends up maintaining your code is a violent psychopath who knows where you live.Martin Golding
> 

As I touched on earlier, there’s a similarity between the ‘other’ audience and the reflexive. The primary distinction is drawn around the proximity to the source. The other does not have the advantage of having authored the code, and therefore doesn’t have the native’s understanding of the underlying logical organization of the source. This disadvantage can be overcome via composing in such a manner that the meaning is emergent from the design. Even if it’s too complex to be obvious, good documentation can address this particular deficiency.

Ultimately, much of software design is about modeling. Software that solves a particular problem should emulate the nature of the challenge it’s attempting to address. If this can be achieved, it’s significantly easier for those who understand the problem to figure out how the software works. Therefore, good design can help improve the effectiveness of a given piece of source to communicate its intent. Along a similar vector, if the design is similar to code that solves a particular issue, it’s easier to understand. As an example, a friend recently asked for feedback about an interface that he’d designed. It loaded a save game file for StarCraft 2. This is what he came up with:

```ruby
replay_file = File.new("spec/fixtures/1v1-game1.sc2replay")
@replay = SC2Refinery::Parser.parse(replay_file)
```

However, Ruby already has several kinds of code in its standard library that loads some information from disk and parses it into some kind of data structure that you can use in your program. The JSON, YAML, and Marshall classes already use a set of methods to import and export data, and they’re named `load` and `dump`, and they’re part of the class directly. Also, in this case, the user of the code shouldn’t need to deal with the creation of a file, since it’s unreasonable to assume that a game replay would come from any other source. Therefore, after some discussion, he adopted the following interface instead:

```ruby
@replay = SC2Refinery.load("spec/fixtures/1v1-game1.sc2replay")
```

This is much nicer to use, and is much simpler. While it may not seem like a whole lot, when rules like this are applied across an entire codebase, they can significantly increase understanding. Multiple reductions of mental overhead add up quickly.

My new favorite trick for adding a little bit of modeling that significantly reduces overhead for the user is the Presenter Pattern. Jeff Casimir demonstrated this very clearly in his presentation at RailsConf 2011, “[Fat Models Aren’t Enough](http://dl.dropbox.com/u/69001/Fat%20Models%20Aren%27t%20Enough%20-%20RailsConf.pdf)”. Here’s a slightly modified example. Imagine that we have a system that manages students, and we’d like to display a report card for them. We might start with some code that looks like this:

```ruby
student = Student.find(options[:student_id])
term = Term.find(options[:term_id])
report_type = ReportType.find(options[:report_type])

puts "#{student.last_name}, #{student.first_name}"
puts "#{report_type.report_title} for #{term.start_date} to #{term.end_date}"
student.courses.each do |course|
  course_report = student.report_data_for_course(course)
  puts course_report.to_s
end
```

Honestly, until this past week, this is the exact code that I would have written. But it turns out that we can do better. Basically, we’re displaying some information that comes from a combination of three different objects. If we think about it some more, we’re really trying to display a report card. So let’s make an object that represents the card, and delegates to its sub-objects. It will then know how to display itself.

```ruby
class ReportCard
  delegate :start_date, :end_date, :to => :term
  delegate :first_name, :last_name, :courses, :report_data_for_course, :to => :student
  delegate :report_title, :to => :report_type

  def initialize(params)
    @student = Student.find params[:student_id]
    @term = Term.find params[:term_id]
    @report_type = ReportType.find params[:report_type_id]
  end

  def student_name
    [last_name, first_name].join(", ")
  end

  def title
    "#{report_title} for #{start_date} to #{end_date}"
  end

  def course_reports
    out = ""
    courses.each do |course|
      out += report_data_for_course(course)
    end
    out
  end
end
```

Now, this is a lot of code. However, as you can see, it’s all focused on composing the information and exposing an interface that makes sense for a report card. Using it is super easy:

```ruby
report = ReportCard.new(options)
puts report.student_name
puts report.title
puts report.course_reports
```

Bam! It’s incredibly obvious. This code is much more clear than before. We’ll see if I’m still as hot on this pattern as I am now in a few months, but I feel the extra object adds a significant amount of clarity.

If the model is too hard to create, or if additional clarity is needed, documentation in the form of comments can also help to improve the understanding of the ‘other’ audience. Comments can be a difficult form of prose to write, because they need to be written at the correct level of abstraction. If they simply repeat what the code does, they’re useless, and if they’re too high-level, certain details and semantics may not be made clear.

Individual bits of code can also be made more clear by developing a narrative within any particular method that’s being written. Telling a story with code may not be something you’ve considered before, but it’s really about maintaining a proper flow in the actions your code is taking. For example, if there’s a bunch of error handling strewn about inside of a method, it’s less clear than bunching all of the error handling near the end. Most code should be an act in three parts: input, processing, and output. If these three parts are mixed together, it can appear much more complicated.

### The forgotten audience: end-users

> If I asked my customers what they wanted, they’d have told me, “A faster horse.”Henry Ford
> 

In the end, all software is used by someone. Use-value is the driving force of virtually all code. Code that doesn’t do anything may be making some kind of important philosophical statement, but isn’t really the sort that I’m talking about.

The introduction of a user imposes significant restrictions upon the way that code is composed. End-users do not need to understand the code itself, but they do need to be able to understand its external interfaces. These needs place an imposition on the way that the code needs to be written, because it *must* address this issue of interface. Sometimes, interface requirements can create a burden on the internal implementation. Needing to support certain behaviors and forms can create complexity for an implementor.

Documentation created for end users must be completely different than that which is created for those inspecting the code itself. Most end-users will not be literate in the arts of software development, and so approach the software object in an entirely different way than those who write code do. Yet, the same semantics must be passed on to them, but at a higher level. There’s a strong movement within the community to start designing software with this kind of end-user documentation in mind, called [README driven development](http://tom.preston-werner.com/2010/08/23/readme-driven-development.html). There are advantages to thinking on this level when beginning, but a nice effect of doing it first is that you can ensure it gets done. A surprising amount of software has poor documentation for its users, because it’s created after the software is finished, and at that time there’s intense pressure to ship it out the door. Writing down information for the end user first ensures that it’s done properly, that all development works in accordance with the documentation, and that all of the use-cases for an end-user have been thought of and are being addressed.
