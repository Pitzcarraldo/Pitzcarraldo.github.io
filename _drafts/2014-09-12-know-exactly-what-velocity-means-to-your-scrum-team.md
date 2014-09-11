---
layout: post
title: "[번역] 당신의 스크럼팀을 위한 벨로시티의 정확한 의미"
description: "[번역] 당신의 스크럼팀을 위한 벨로시티의 정확한 의미"
modified: 2014-09-12
category: articles
tags: [Agile, Scrum, Velocity, 애자일, 스크럼, 벨로시티, 추정]
comments: true
share: true
---
# 당신의 스크럼팀을 위한 벨로시티의 정확한 의미
[원문] [Know Exactly What Velocity Means to Your Scrum Team](http://www.mountaingoatsoftware.com/blog/know-exactly-what-velocity-means-to-your-scrum-team) by Mike Cohn

본문은 원래 Mike Cohn의 월간 뉴스레터에 실렸던 글입니다. 만약 이 글이 맘에 든다면, [이곳](http://mountaingoatsoftware.us4.list-manage.com/subscribe?u=69aab24b249d49f4eade10a9c&id=0b477a3c16)에서 메일링에 가입해주세요. 블로그에 포스팅 되기 전에 글을 배달 받을 수 있습니다.

When talking about it informally, I define velocity as simply a measure of how fast a team is going. And for most purposes, this definition works quite well. However, it creates confusion on some of the finer points of what should count in calculating a team's velocity. This confusion comes about because there are really two more precise ways of defining velocity. Let's see what they are.

비공식적으로 벨로시티에 대해 이야기 할 때, 나는 그것을 단순이 팀이 얼마나 빨리 움직이는지에 대한 척도로 정의합니다. 대부분의 경우에 이 정의는 잘 적용 됩니다. 그러나 팀의 속도를 계산하는 계산해야하는지의 미세한 점의 일부에 혼란을 만듭니다. 이 혼란이 속도를 정의하는 두 가지 더 정확한 방법이 정말 있기 때문에에 대해 제공됩니다. 의 어떤 정보가 있는지 알아 봅시다. 


1) Velocity measures how much functionality a team delivers in a sprint.
2) Velocity measures a team's ability to turns ideas into new functionality in a sprint.

Those may sound the same. They are subtly different. To see how, suppose you hop in a river and begin swimming. After an hour, you measure how far you've traveled, and you are 2 kilometers from where you began. In agile terms, we might want to call this your velocity and say you swim 2 kilometers per hour or per sprint, if a swimming sprint is an hour long.

But, what if the river had been flowing at the rate of one kilometer per hour against you while you swam? In that case, you really swam 3 kilometers. Measured against the banks of the river, you've only moved two kilometers of physical distance. But while going forward two kilometers, you overcame being pushed backward one kilometer by the river.

![swimmer](http://www.mountaingoatsoftware.com/uploads/blog/swimmer.jpg)

So, is your velocity two or three? If we use our first definition—velocity is how much a team delivers in a sprint—then velocity is two. This swimmer delivered 2 kilometers of progress.

If we use our second definition—velocity is the ability to turn ideas into new functionality—then velocity is three. This swimmer has the ability to deliver 3 kilometers of progress per sprint, and we'd see that he was swimming in a lake with no current instead of a river.

To see how this applies to an agile project, consider the issue of whether a team should earn velocity credit for fixing bugs during a sprint. A team that uses velocity to measure how much functionality is delivered in a sprint will not claim credit for bug fixes. No new functionality has been delivered. So no points are earned.

A team using defining velocity as the ability to turn ideas into functionality, on the other hand, will claim credit for bug fixes. Their logic is that the time spent on bug fixing could have been spend adding new functionality except the product owner prioritized different work for them.

For many teams, the two definitions will yield the same value. Values will differ most for teams doing work for which they are not taking velocity credit--usually teams doing things like a lot of bug fixing or doing large amounts of refactoring.

Neither of these subtle differences in the definition of velocity is always better than the other. The one you use should largely depend on what you hope to learn by measuring it and by your expectations about the future.

If you expect the future to be just like today—that is, the team will spend the same amount of time doing bug fixing, refactoring and the like as they do now, then using velocity as a measure of how much forward progress is made will be the right answer for you.

However, if you expect the future to be different—perhaps the large refactoring and time spent fixing bugs will soon be over—then you may want to define velocity as the team's ability to turn ideas into functionality, and would then add to velocity the points given to those activities.

The most important thing is to clarify with everyone on the team, including the product owner and ScrumMaster, is exactly what your team means when they use the term “velocity.” Having a precise definition makes it very easy to answer questions that come up around what should be counted when measuring velocity.


1) 속도 측정 팀은 스프린트에서 제공 얼마나 많은 기능을 제공합니다. 
2) 속도는에는 스프린트의 새로운 기능에 대한 아이디어를 켤 수있는 팀의 능력을 측정합니다. 

이들은 동일한 소리있다. 그들은 미묘하게 다릅니다. 어떻게 확인하려면 당신이 강물에 뛰어 수영을 시작한다고 가정합니다. 한 시간 후, 당신은 당신이 여행을 얼마나 멀리 측정, 당신은 당신이 시작된 곳에서 2km이다. 민첩한 측면에서, 우리는 당신의 속도를 호출 할 및 수영 질주 긴 시간 인 경우, 시간 당 또는 스프린트 당 2km 수영 말할 수 있습니다. 

당신이 수영 반면, 강 당신에 대해 시간 당 일km의 비율로 무엇을 흐르는 된 경우? 이 경우, 당신은 정말 3km를 수영. 강 유역에 대하여 측정, 당신은 단지 물리적 거리의 이km를 이동했습니다. 앞으로 이km가는 동안, 당신은 일km 강에 의해 뒤로 밀려 극복했다. 

! [수영] (http://www.mountaingoatsoftware.com/uploads/blog/swimmer.jpg) 

그래서, 당신의 속도를 두 개 또는 세입니까? 우리가 사용하는 경우 우리의 첫 번째 정의 - 속도는 팀이 인 스프린트 - 다음 속도에서 제공하는 가격에 해당합니다. 이 수영 진행 2km를 전달했다. 

우리가 사용하는 경우 두 번째 정의 - 속도가 새로운 기능 - 다음 속도로 아이디어를 할 수있는 기능은 세 가지입니다. 이 선수는 스프린트 당 진행 3km를 제공 할 수있는 능력을 가지고 있으며, 우리는 그가 대신 강 전류가와 호수에서 수영하는 것을 볼 것. 

이 애자일 프로젝트에 적용하는 방법, 스프린트 동안 버그를 수정을위한 속도 학점을 취득해야하는지 여부를 팀의 문제를 생각해 볼 수 있습니다. 속도를 사용하는 팀은 버그 수정에 대한 크레딧을 주장하지 않습니다 스프린트에 전달 얼마나 많은 기능을 측정합니다. 새로운 기능이 제공되지 않았습니다. 그래서 더 포인트는 적립되지 않습니다. 

다른 한편으로는, 기능에 아이디어를 할 수있는 능력으로 정의 속도를 사용하는 팀은 버그 수정에 대한 크레딧을 주장하는 것입니다. 이들 논리는 시간이 제품 소유자 제외한 새로운 기능을 추가하고 소비를 가질 수 버그 수정에 소요 그들을 위해 다른 작업을 우선한다는 것이다. 

여러 팀,이 정의는 동일한 값을 산출 할 것이다. 일반적으로 버그 수정이 많이 같은 일을하는 팀이나 리팩토링 많은 양의 일을 - 값은 속도 크레딧을 복용하지 않은 일을하고 팀을 위해 가장 다릅니다. 

속도의 이러한 정의에 미묘한 차이 중 어떤 것도 다른 것보다 항상 더 낫다. 당신이 주로 당신이 그것을 측정하고 미래에 대한 기대에 의해 배울 희망에 따라 달라한다 사용하는 일. 

당신은 미래가 바로 오늘, 즉, 팀은 버그 수정, 리팩토링 등을하고 같은 시간을 보낼 것처럼 될 것으로 예상 한 경우에는 다음 만들어 조금씩 앞으로 진보의 척도로 속도를 사용하여, 지금 같은 것 당신을위한 올바른 해답이 될 수. 

당신이 기대하는 경우, 미래는 - 다른 아마도 지도록하기 위해서는 기능에 대한 아이디어를하는 팀의 기능과 같은 속도를 정의 할 수 있습니다 이상 - 다음 큰 리팩토링 시간 동안 고정 버그는 곧있을 것 한 다음 속도에 포인트를 추가합니다 이러한 활동에 부여. 

가장 중요한 것은 그들이 용어를 사용할 때 팀이 의미 정확히 무엇이며, 제품 소유자와 스크럼 마스터를 포함하여 모든 팀원과 명확하게하는 것입니다 "속도."정확한 정의는 매우 쉽게 주변에 올 질문에 대답 할 수 데 속도를 측정 할 때 어떻게 계산해야합니다.
