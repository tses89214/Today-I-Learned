## Metadata
- Reading Date: 2026-02-02
- Post: [Stop building dashboards: what high-impact data teams do instead](https://medium.com/dashboards-suck/stop-building-dashboards-what-high-impact-data-teams-do-instead-c36290240461)
- Posted Date: 2025-11-24
- Tags: Infra, DashBoard

## Key Takeaways
The author indicated that most data teams works as "Data Helpdesk", building dashboards for stakeholder, satisfying their needs and focus on stakeholder's happiness, not business impact.

You should treat data teams as a "product team", focus on decision-making, only excellent decisions can lead to success, not the dashboard you shipped. 

So, you need to be prepared to disappoint some of your stakeholders because your target is not to fulfill their needs.

Author mentioned three dysfunction that affect data team the most:
1. Solution-first thinking<br>
Solution-first thinking means we like to think newest and advanced technique first, but don't care about "Who and How will use it to make decision?". But this is the KEY question that decide this model will create business values or not.
<br>Take "churn prediction" as example, if we use "solution-first thinking", an idea will be raised:"how about train an ML model to predict it?"
And the whole team spend three weeks to build a prediction system, but the customer success manager still use its experience to do make decision because the prediction need three weeks to run and s/he can not understand it.
<br>Maybe, we can just use a simple cohort analysis which only need few hours to finish it, and much easier for user to understand. 
<br>The takeaway is: Think your user first, not your solution.

2. Technical purity over pragmatic impact<br>
Sometimes we will value code quality, architecture over its practical results and impacts. The author gave an example that the data scientist spent six months to build an accurate system with 87% accuracy, but it influenced 0 business decision. On the other hand, sales team solved their business problem merely with Google Sheet which has complex and hard-to-maintained formulas. The author describes this situation like giving an F1 race car to a person who just only want to buy some groceries.


3. Too far from consequences<br>
Data team usually report to CTO or CDO, who will emphasize on code quality and system structure. But the correct way is data team should report to business leaders who can actually make business decision.
<br>Wrong organization structure will turn data team into a consulting startup in company. Team members will focus on beautiful presentation and analysis report, and after that, everybody moved on to the next project.


In the end, they put company's strategy higher than personal needs, and developed a "dual conversation track system": their can provide support to personal requests, but take more emphasis on how these data can affect decision and alias to company's values. And also they put more effort on support business headers or person who can make decision.


# ----------------- AI Generated ------------

## Key Takeaways
The author argues that many data teams function merely as a "Data Helpdesk," prioritizing stakeholder satisfaction and dashboard delivery over actual business impact. To drive real value, data teams must operate as "Product Teams" focused on decision-making. Since success is measured by the quality of decisions—not the number of dashboards shipped—teams must be prepared to disappoint some stakeholders to stay focused on high-impact goals.

The author identifies three major dysfunctions affecting data teams:

1. Solution-First Thinking This occurs when teams prioritize advanced techniques without asking: "Who will use this, and how will it drive a decision?"

Example: A team might spend three weeks building a complex ML model for "churn prediction" that a manager ignores because it's too slow or confusing.

The Lesson: A simple cohort analysis completed in hours is often more valuable than a complex model that no one uses. Think about the user first, not the solution.

2. Technical Purity Over Pragmatic Impact Data professionals often overvalue code quality and elegant architecture at the expense of practical results.

Example: A data scientist might spend six months on a model with 87% accuracy that influences zero decisions, while a sales team solves a real problem using a "messy" Google Sheet.

The Lesson: Building an over-engineered system for a simple task is like giving an F1 racing car to someone who just needs to buy groceries.

3. Isolation from Consequences Data teams often report to a CTO or CDO, who focuses on technical excellence. However, the author suggests they should report to business leaders who own the decisions. Without this alignment, data teams risk becoming an "internal consulting startup"—producing beautiful reports and presentations, but moving on to the next project without ever seeing the real-world consequences of their work.

#### Conclusion
To succeed, data teams must align with company strategy over individual requests. By implementing a "Dual Conversation Track," teams can still provide basic support while shifting their primary focus toward the key decision-makers who drive the company’s core values.