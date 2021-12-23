---
layout: default
title: Getting Help
permalink: /help/
---

### Online Help
1. You can ask questions any time on our Piazza forum
1. For questions about course material, please ask public questions
in case others have the same question. For questions about your grade
you can ask a private question of the Instructors
1. If you ask an anonymous question, your identity will be anonymous
from classmates, but not from me or the TAs
1. We prefer Piazza to email
1. Students are welcome to help each other on Piazza
1. For debugging questions, we prefer Github links to screen shots

### Drop-In Office Hours

{% for person in site.people -%}
| [{{person.name}}](mailto:{{person.email}}) ({{person.role}}) | {{person.office_hours}} |
{% endfor %}

- If our regular office hours don't match your schedule, please contact us on Piazza to make an appointment at a mutually available time.

### CS Tutoring Center

The [Computer Science Tutoring Center](https://tutoringcenter.cs.usfca.edu/) is a great resource to get help from other students.
