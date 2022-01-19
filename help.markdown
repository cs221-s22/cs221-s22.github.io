---
layout: default
title: Getting Help
permalink: /help/
---

### Online Help
1. You can ask questions any time on [CampusWire](https://campuswire.com/c/G4A7B9CEF/feed)
1. For questions about course material, please ask in the feed in case
others have the same question
1. For questions about your grade please DM the person who graded your work
1. Anonymous questions hide your name from classmates, but not from me or the TAs
1. We prefer CampusWire to email
1. Please help each other on CampusWire so it's a valuable forum for all

### Drop-In Office Hours

{% for person in site.people -%}
| [{{person.name}}](mailto:{{person.email}}) ({{person.role}}) | {{person.office_hours}}) |
{% endfor %}

If our regular office hours don't match your schedule, please contact us on CampusWire to make an appointment at a mutually available time.

### CS Tutoring Center

The [Computer Science Tutoring Center](https://tutoringcenter.cs.usfca.edu/) is a great resource to get help from other students.
