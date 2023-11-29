## 한국어 번역
---
좋아, 어떻게 진행되고 있나요? 컴퓨터 과학 61A에 오신 것을 환영합니다. 이 세계에서 가장 좋은 컴퓨터 과학 수업입니다. 우리는 Scheme이라는 프로그래밍 언어를 사용할 것입니다. 그러나 이 강좌는 Scheme에 관한 수업이 아닙니다. 우리는 STk라는 특정한 Scheme 구현을 사용합니다. 우리는 STk에 식을 입력할 수 있고, 그것은 우리에게 답을 제공할 것입니다. Scheme은 전위 표기법을 사용하며, 연산자가 피연산자보다 먼저 나오는 특징이 있습니다. 이로써 인자의 수에 상관없이 함수를 가질 수 있습니다. 예를 들어, 우리는 단순히 (+ 6 8 2 99 4000)를 작성함으로써 6, 8, 2, 99 및 4000의 합을 계산할 수 있습니다.

일부 연산자는 곱셈과 같이 항등 원소인 1을 가지고 있습니다. 따라서 아무 것도 곱하지 않으면 결과는 1이 됩니다. 그러나 모든 함수가 가변 인자를 가지는 것은 아닙니다. 예를 들어, 우리는 인자 없이 나눗셈 함수를 호출할 수 없습니다.

우리는 'quote' 함수를 사용하여 값이 기호 또는 단어인 식을 만들 수 있습니다. 예를 들어, (quote hello)는 우리에게 기호 'hello'를 제공합니다. 또한 함수를 사용하여 단어와 문장을 조작할 수 있습니다. 예를 들어, 함수 (combine hello world)는 단어 'hello'와 'world'를 이어서 새로운 단어를 형성합니다. 함수 (sentence hello world)는 'hello'와 'world'라는 단어로 문장을 만듭니다.

또한 'define' 키워드를 사용하여 자체 프로시저를 정의할 수 있습니다. 예를 들어, 우리는 x라는 인수를 가지고 그 제곱을 반환하는 'square'라는 프로시저를 정의할 수 있습니다. 'square' 프로시저를 인수와 함께 호출하면 인수가 프로시저의 본문에 대체되고 식이 평가됩니다.

Scheme에서는 일반적인 프로시저 호출 규칙에 몇 가지 예외가 있습니다. 이 중 하나는 "define" 키워드입니다. "define"를 사용하여 변수에 값을 할당할 때 해당 값은 즉시 평가되지 않습니다. 이것은 변수에 값을 할당하기 전에 변수를 호출하려고하면 오류가 발생한다는 것을 의미합니다. 이는 "define"이 Scheme에서 특별한 형태임을 나타냅니다.

Scheme에는 약 12개의 특수 형태가 있으며, "define"은 학습하는 첫 번째 것입니다. 이번 주에는 몇 개가 더 소개될 것입니다.

이제 이 강좌의 몇 가지 행정적인 사항에 대해 이야기 해 봅시다. 여러분은 방금 전달 받은 것이 있어야 할 핸드아웃이 필요합니다. 곧 다른 핸드아웃도 받게 될 것입니다. 또한 나중에 논의할 컴퓨터 계정 양식을 작성해야 합니다.

이 강좌를 위해 구입해야 할 자료가 몇 가지 있습니다. 가장 중요한 것은 교재입니다. 사용된 것을 구매해도 좋지만, 두 번째 판이라는 것을 확인하십시오. 또한 강의 노트와 Scheme 참조 매뉴얼, 일부 샘플 시험이 포함 된 강의 노트가 있는 큰 부피도 있습니다. 강의 노트를 사용하여 따라가고 필요한 경우에는 메모를 작성하십시오. 참조 매뉴얼은 Scheme 구문에 대한 정보를 찾는 데 유용합니다.

이제 섹션 과제에 대해 이야기해 봅시다. 모든 랩 섹션은 해당 토론 섹션에 연결되어 있으며, 이러한 섹션 내에서 작은 그룹으로 작업하게 됩니다. 중요한 것은 여러분이 개설된 섹션에 참석해야 한다는 것입니다. 그렇지 못한 경우 다른 학생과 교환을 시도할 수 있습니다. 그러나 주의하세요. TA가 섹션에 등록되어 있는지 확인하기 전까지 어떤 양식에도 서명하지 않을 것입니다.

마지막으로, 이 강좌에서는 부정행위를 하지 않는 중요성을 강조하고 싶습니다. 협력은 장려되지만 함께 작업할 수 있는 한계가 있습니다. 부정행위는 동료 학생에게만 불공평한 것뿐만 아니라 학습 과정을 약화시킵니다. 그러니 성실하게 일하고 그룹 구성원을 지원하기 위해 최선을 다해 주시기 바랍니다.

곡선은 있지만 등급은 곡선을 따라가지 않는 것이 좋습니다. 왜냐하면 그렇게 하면 서로 경쟁하는 것이 아니라 협력하는 것이기 때문입니다. 다른 오해 중 하나는 이 강좌에서 부정행위를 하면 캘리포니아 대학의 평판에 해를 끼칠 것이라는 것입니다. 이런, 세 달마다 어떤 축구 선수가 무언가를 하긴 하지만 캘리포니아 대학의 평판은 꽤 괜찮습니다. 그것이 부정행위를 하지 말아야 하는 이유가 아닙니다. 부정행위를 하지 말아야 하는 이유는 다음과 같습니다.

1. 당신은 평생 동안 될 사람을 만들고 있습니다. 인간 행동은 대부분 습관의 문제입니다. 근본적인 수업에서 꼬리를 짤 버린 버릇을 들이면 어떻게 하겠습니까? 그리고 실제로 직장에 입성하고 옆에 앉은 사람이 동일한 일을 하지 않을 때 어떻게 할 것인가요? 당신은 어떤 누군가의 어깨 너머를 볼 수 없을 것입니다. 하지만 당신은 꼬리를 짤 방법을 찾을 것입니다. 나는 이 수업에서 부정행위를 한 사람이 프로그램을 작성한 비행기에 탑승하고 싶지 않습니다.
2. 부정행위에서 어떤 최선의 결과가 나올 수 있을까요? 당신은 스스로 알지 못하고 싫어하는 일을 평생 하는 삶으로 내몰립니다. 그러니 부정행위를 하지 마세요, 만약 그렇게 한다면, 당신은 정말로 스스로를 해치고 있습니다.

이 수업에서 부정행위를 피하는 방법은 간단합니다. 무언가를 이해하지 못하면 그 다음 주까지 기다리지 마세요. 교수님을 귀찮게 생각하지 마세요. 오피스 아워에 와서 도움을 요청하세요. 그게 제 역할입니다.

기타 행정적인 사항:

- 섹션 20과 120에 참여 중인 경우, 랩 및 토론 시간이 서로 반 시간 차이 나게 설정된 것에 대해 사과드립니다. 긴 이야기가 있지만, 최선을 다한 결과입니다.
- 처음에 언급된 CSUA 도움 세션은 이 핸드아웃의 맨 끝에 있습니다.

이제 몇 가지 용어에 대해 알아보겠습니다:

형식 매개변수는 함수에 대한 인수의 이름입니다. 함수의 본체는 정의되는 곳입니다. 실제 인수 표현식은 함수에 대한 입력이며, 실제 인수 값은 입력의 특정 값입니다. 대부분의 경우, 어떤 것을 참조하는지 명백하지만 중요할 때는 명확하게 알려드릴 것입니다.

질문을 하기 위해서는 이 웹캐스트 중이므로 손을 올리는 것을 보지 못할 수 있습니다. 따라서 질문이 있으면 손을 흔들거나 필요하다면 소리를 지르세요.

여기에 여러분이 정의할 수 있는 전형적인 함수가 있습니다:

arduinoCopy code

`(define (plural word) (if (equal? (last word) 'y) (word (butlast word) 'ies) (word word 's)))`

이제 함수의 문제를 해결해 봅시다:

arduinoCopy code

`(define (plural word) (if (equal? (last word) 'y) (word (butlast word) 'ies) (word word 's)))`

그리고 오늘은 여기까지입니다. 행정적인 질문이 있으면 금요일에 물어보세요. 이 핸드아웃에 답이 있다면 당신은 저에게 1달러를 빚지게 됩니다. 그러나 질문이 어리석은 것일지라도 과목 내용에 대해 질문하지 말라는 일은 없습니다. 어리석은 질문이 종종 많은 다른 사람들이 가지고 있는 질문입니다. 따라서 모두에게 유익합니다.

이렇게 하면 단어 중간에 문자가 나타납니다. 네, 맞아요. 첫 번째에서 두 번째로 가져옴으로써 두 번째 문자를 찾았습니다만, 첫 번째에서 가져오기 때문에 그것들의 조합을 만들 수 있습니다. 그러기 위해서, 이것으로 만들어진 또 다른 것도 있습니다. 봅시다, 어, 이건...아니요, 이런 식입니다. 어, 아니에요. 와, 이것은 흥미롭군요. 아마 내가 틀렸을 수도 있습니다. 아마 없을 수도 있습니다. 어, 단어에 대해서는 좋은 질문입니다. 음, 여기에 아이템이 있습니다. 그걸 시도해 봅시다. 아이템, 어, '컴퓨터'에 대한 인덱스를 찾아 봅시다. 네, 여기에 있네요. 아이템. 음, 네, 단어와 문장에 대한 모든 것에 대한 정보를 기억해 주셔서 감사합니다. 이것이 바로 내가 당신의 질문에 손을 교차하고 있었던 이유입니다. 단어와 문장에 관한 모든 것은 이 강좌를 위해 버클리에서 Scheme에 추가한 내용입니다. 보통의 Scheme에서는 참조 매뉴얼과 같은 곳에 단어와 문장이라는 것이 존재하지 않습니다. 그들은 텍스트를 다르게 다룹니다. 이 방법이 더 나아요. 몇 주 후에는 그 기반이 무엇인지 알아볼 것입니다. 하지만 지금은 이 방법으로 생각하는 것이 훨씬 더 쉽습니다. 단어와 문장에 대한 아이디어는 로고라는 언어에서 왔습니다. 로고로 프로그래밍을 해 본 적 있나요? 음, 몇 명이네요. 예전에는 더 많았습니다. 학기가 끝날 때쯤에는 로고 인터프리터를 작성할 때 잘 알게 될 것입니다. 어쨌든, 우리는 그곳에서 단어와 문장이라는 개념을 가져왔고, 그것이 삶을 좀 더 쉽게 만듭니다. 네, 계속하죠. 음, 아마 이걸 하자. 이런 식이죠, cd lectures 1.1. 예, 응. 그의 질문은, 나만의 작은 리더를 작성해서 아포스트로피를 빼 놓을 수 있을까요? 그렇게 하지 않는 것이 좋습니다. 때로는 이 심볼의 값이나 이 프로시저 호출의 값이 무엇인지 알고 싶을 때가 있습니다. 그래서 그것을 만들어내는 것이 좋습니다. 이것은 단순한 문법적인 차이가 아니라 기본적으로 프로그램과 데이터의 차이입니다. 정말 그렇게 생각하지 않는 것이 좋습니다. 그럼, 여기는 Pig Latin 프로그램이 있습니다. Pig Latin을 구사하는 사람이 있나요? 오 마이 갓, 요즘에 학교에서 뭐 가르칩니까? 좋아요, Pig Latin이죠. 어릴 때는 부모님에게 비밀 언어로 쓰이던 언어였습니다. 아마 요즘에는 부모님이 자녀에게 비밀 언어로 사용하고 있을 것입니다. 작동 방식은 단어를 Pig Latin으로 번역하는 것인데, 모든 처음 자음을 끝으로 옮기고 "ay"를 더합니다. 그래서 "pig" + "le" = "ig-pay"가 됩니다. 그러면 이 프로그램이 어떻게 이를 수행할까요? 함께 살펴보겠습니다. 여기서는 "pl-done?"이라는 많은 헬퍼 함수를 사용하고 있습니다. 아마 실제로는 필요 이상으로 많은 헬퍼 함수를 사용하고 있습니다. 제게 주어진 경우 나는 "pl-done?"이라는 것을 갖지 않을 것 같습니다. 그냥 이 식을 여기에 놓을 것 같아요. 그러나 이 작동 방식을 가르치기 위해 "pl-done?"이라는 프레디케이트 함수가 있습니다. 이 함수는 단어를 가져와 "이 단어의 첫 글자가 모음인가?"라는 질문을 합니다. 우리는 "word"의 "first"를 취하고 그것이 "vowel?"에 의해 참 또는 거짓인지를 물어봅니다. 그럼 "vowel?"은 어떻게 작동합니까? 이것은 내장 함수가 아닙니다. 그래서 "member?"는 내장 함수입니다. 우리는 "이 문자가 'aeiou'의 멤버인가?"라고 물어봅니다. 그렇다면 그것은 모음이므로 주어진 단어에 'y'를 추가합니다. 그렇지 않으면 여기가 재미있는 부분입니다. 첫 번째 글자를 제외한 모든 글자를 가져오고 그 후에 첫 번째 글자를 붙입니다. 그래서 주어진 "scheme"이라면 "ch-s"로 변합니다(맨 끝으로 's'를 옮김). 그러나 이것이 원하는 답이 아니기 때문에 "pig"를 그것에 대해 묻습니다. 그래서 여기 전체 식은 "pig-latin" of "word" but "first" of "word"입니다. 이렇게 함으로써 "ch-s"에 대한 "Pig Latin of 'ch-s'"를 계산하게 되고 이것은 "Pig Latin of 'he'"를 계산하게 되고 이것은 "Pig Latin of 'im'"을 계산하게 됩니다. 그리고 그것은 "아하, 이제 모음으로 시작합니다. 끝에 'y'를 붙이겠습니다"라고 말합니다.

여

# 대기 목록에 오신 것을 환영합니다

현재 대기 목록에 있으며 여러분의 인내를 감사드립니다. 섹션 20 또는 21에 참여하게 될 것으로 예상됩니다. 계속 주시하시고, 해결되면 알려드리겠습니다.

금요일에 뵙기를 기대합니다. 이해와 지원에 감사드립니다!


## 영어 원문
---
All right, how are we doing? Welcome to Computer Science 61A, the best computer science class in the world. We will be using a programming language called Scheme, but this is not a course about Scheme. We are using a particular implementation of Scheme called STk. We can type expressions into STk and it will give us the answer. Scheme uses prefix notation, where the operator comes before the operands. This allows us to have functions with any number of arguments. For example, we can calculate the sum of six, eight, two, ninety-nine, and four thousand by simply writing (+ 6 8 2 99 4000).

Some operators, like multiplication, have an identity element, which is one. So, if we multiply nothing, the result is one. However, not all functions have variable numbers of arguments. For example, we cannot call the division function with no arguments.

We can create expressions whose values are symbols or words by using the quote function. For example, (quote hello) gives us the symbol hello. We can also use functions to manipulate words and sentences. For example, the function (combine hello world) concatenates the words hello and world to form a new word. The function (sentence hello world) creates a sentence with the words hello and world.

We can also define our own procedures using the define keyword. For example, we can define a procedure called square, which takes an argument x and returns the square of x. When we call the square procedure with an argument, the argument is substituted into the body of the procedure and the expression is evaluated.

For Scheme, there are a few exceptions to the ordinary procedure calling rule. One of these exceptions is the "define" keyword. When you use "define" to assign a value to a variable, the value is not immediately evaluated. This means that if you try to call the variable before giving it a value, you will get an error. This is because "define" is a special form in Scheme.

There are about a dozen special forms in Scheme, and "define" is the first one you are learning. There will be a couple more introduced as the week progresses.

Now, let's talk about some administrative things for this course. You will need a handout, which you should have just received. You will also receive another handout shortly. Additionally, you will need to fill out a computer account form, which we will discuss later.

There are some materials you will need to purchase for this course. The most important one is the textbook. You can buy it used if you prefer, but make sure it is the second edition. There is also a course reader, which comes in two volumes. You will only need to buy the new volume one, as volume two does not change from semester to semester.

The big volume contains my lecture notes, as well as the Scheme reference manual and some sample exams. You should use the lecture notes to follow along and take notes if necessary. The reference manual is useful for looking up information about Scheme syntax.

Now, let's discuss the section assignments. Every lab section is linked to a corresponding discussion section, and you will be working in small groups within these sections. It is important that you attend a section that has openings. If you are unable to do so, you can try to make a trade with another student. However, please note that I will not sign any forms until a TA confirms that you are enrolled in a section.

Lastly, I want to emphasize the importance of not cheating in this course. While collaboration is encouraged, there are limits to how much you can work together. Cheating is not only unfair to your fellow students, but it also undermines the learning process. So please, do your best to work honestly and support your group members.

A curve, but it's not grading on a curve is evil because it makes you compete with each other instead of cooperating. So you are not hurting anybody else by doing well in the class. Another misconception is that you are going to harm the reputation of the University of California if you cheat. Now come on, every three or four years some football player does something terrible, but the reputation of the University of California is pretty good. That's not why you shouldn't cheat. Here's why you shouldn't cheat:

1. You are constructing the person you're going to be for the rest of your life. Human behavior is mostly a matter of habits. If you get in the habit of cutting corners early in your career, how are you going to make it through the harder upper division classes? And what are you going to do when you actually get a job and the person next to you isn't doing the same thing? You won't be able to look over somebody's shoulder, but you will find ways to cut corners. And I don't want to fly in an airplane that was programmed by somebody who cheated in this class.
2. What's the best thing that can come out of cheating? You condemn yourself to a life of doing something you don't know how to do and don't like doing. So don't cheat, if you do, you're really hurting yourself.

The way to avoid cheating in this class is simple. If you don't understand something, don't wait till next week to see if it gets better. Don't think you're bothering the professor. Come to office hours and ask for help. That's what I'm here for.

Other administrative things:

- If you're in section 20 and 120, I apologize for the lab and discussion times being half an hour offset from each other. It's a long story, but it was the best we could do.
- The CSUA help sessions mentioned at the beginning are at the very end of this handout.

Now, let's address some vocabulary:

A formal parameter is the name for an argument to a function. The body of a function is where the definition is. The actual argument expression is the input to the function, and the actual argument value is the specific value of the input. In most cases, it's obvious which one is being referred to, but when it matters, we will be clear about it.

Regarding asking questions, since this is being webcast, I might not be able to see your hand in the air. So if you have a question, wave your hand around or yell if needed.

Here's a typical function that you might define:

```
(define (plural word)  (if (equal? (last word) 'y)      (word (butlast word) 'ies)      (word word 's)))
```

Now, let's fix the problem with the function:

```
(define (plural word)  (if (equal? (last word) 'y)      (word (butlast word) 'ies)      (word word 's)))
```

And that's it for today. If you have any administrative questions, you can ask them on Friday. If the answer is in this handout, you owe me a dollar. But don't hesitate to ask course content questions, even if you think they might be stupid. Stupid questions are often the ones that many other people have, so it's beneficial for everyone to ask them.

That gives your character in the middle of a word. Well, yeah. Um, we found the second character by taking the first of, but first so you can make compositions of those. To do that, um, there's also something made up of that. Let's see, uh, I think this...wait, no, it works this way. Um, nope. Whoa, that's interesting. Maybe I'm wrong. Maybe there isn't one. Um, for words, good question. Oh, there's item. Let's try that. Item, um, for quote computer. Yeah, there we go. Item. Um, oh yeah, thank you for reminding me all the stuff about words and sentences. This is why I had my fingers crossed in your question. All the stuff about words and sentences is stuff that we added to Scheme here at Berkeley for your benefit in this class. Um, in sort of ordinary Scheme, like in the reference manual, there's no such thing as words and sentences. They deal with text in different ways. This way is better. Um, it's easier. In a few weeks, we'll sort of see what underlies it. Uh, but for now, it's much, much easier to think about it this way. The idea of words and sentences came from a language called Logo. Anybody ever programmed in Logo? Huh, a few people. Um, used to be more than that. Um, you're going to get to know it very well toward the end of the semester when you write a Logo interpreter. Um, but anyway, we've imported this idea of words and sentences from there and it just makes life easier. Um, okay, um, moving on. Oh yeah, let me do this. Whoops, cd lectures 1.1. Yeah, uh-huh. Uh, his question is, can I write my own little reader that lets you leave out the apostrophe? You don't want to because sometimes you really do want to know what's the value of this as a symbol or what's the value of this procedure call. Um, so you really want to make it. It's not just a stupid syntactic distinction. It's the difference between program and data, basically. Um, and it really matters. So I wouldn't think that way if I were you. Um, okay, so here's a little Pig Latin program. Who speaks Pig Latin? Oh my God, what do they teach in school these days? All right, Pig Latin. Um, when I was a kid, it was a secret language that kids used to keep secrets from their parents. I guess these days it's a language that parents use to keep secrets from their kids. Um, and the way it works is to translate a word to Pig Latin, you take all the leading consonants, move them to the end, and add "ay". So, "pig" + "le" = "ig-pay". So we take the "sc" everything up to the first vowel, up to but not including, move it to the end, and add a "y". Okay, now how does this program accomplish that? Let's take a look. It says, if "pl-done?" of word, in this example we are using a lot of helper functions, maybe more than you really need. If I were writing this for my own purposes, I'm not sure I would have something called "pl-done?". I might just put this expression up in there. But for purposes of teaching you how this works, we have a predicate function called "pl-done?" that takes a word and it asks the question, "Is the first letter of this word a vowel?" Right? We take "first" of "word" and then we ask "vowel?" true or false. Now how does "vowel?" work? That's not built in, but "member?" is built in. So we're saying, "Is this letter a member of 'aeiou'?" If so, then it's a vowel and so I take the word that I was given and I add a "y" at the end. Otherwise, here's the fun part. I take all but the first letter and then I stick the first letter after that. So if I'm given "scheme", I turn it into "ch-s" (moving the "s" to the end). But that's not the answer I want, so what I do is I ask for "pig" of that. Okay, so the entire expression here is "pig-latin" of "word" but "first" of "word" and that will compute "Pig Latin of 'ch-s'" which will compute "Pig Latin of 'he'" which will compute "Pig Latin of 'im'" which will say, "Aha, now it starts with a vowel. I'll stick a 'y' at the end".

There

# Welcome to the Waiting List

You are currently on the waiting list, and we appreciate your patience. We anticipate that you will be in section 20 or 21. Please stay tuned, and we will inform you if it works out.

We look forward to seeing you on Friday. Thank you for your understanding and support!