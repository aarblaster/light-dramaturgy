---
permalink: /contact
title: "Contact"
author_profile: true
---

This project is being undertaken by me, Anthony Arblaster ([czw527@york.ac.uk](mailto:czw527@york.ac.uk)), who is a doctoral student at the University of York. 

It is being supervised by Dr Katherine Graham ([katherine.graham@york.ac.uk](mailto:katherine.graham@york.ac.uk)) and Dr Rebecca Benzie ([rebecca.benzie@york.ac.uk](mailto:rebecca.benzie@york.ac.uk)).

This project is being conducted according to restrictions that have been subject to approval by the School of Arts and Creative Technologies Ethics committee. The Chair of the ACT Ethics committee can be contacted on [ACT-ethics@york.ac.uk](mailto:act-ethics@york.ac.uk) if you have concerns that haven't been met by Anthony or the Supervisors.

In the first instance, please contact me, I'll be happy to answer all your questions.

<div class="author__urls-wrapper">
  <!-- This is the links in a row -->
  <ul class="author__urls social-icons" style="display: flex; gap: 1rem; list-style: none; padding: 0; margin: 0;">
    {% if author.uri %}
      <li><a href="{{ author.uri }}"><i class="fas fa-fw fa-link icon-pad-right" aria-hidden="true"></i>Website</a></li>
    {% endif %}
    {% if author.email %}
      <li><a href="mailto:{{ author.email }}"><i class="fas fa-fw fa-envelope icon-pad-right" aria-hidden="true"></i>Email</a></li>
    {% endif %}
    {% if author.academia %}
      <li><a href="{{ author.academia }}"><i class="ai ai-academia ai-fw icon-pad-right" aria-hidden="true"></i>Academia</a></li>
    {% endif %}
    {% if author.orcid %}
      <li><a href="{{ author.orcid }}"><i class="ai ai-orcid ai-fw icon-pad-right" aria-hidden="true"></i>ORCID</a></li>
    {% endif %}
    {% if author.github %}
      <li><a href="https://github.com/{{ author.github }}"><i class="fab fa-fw fa-github icon-pad-right" aria-hidden="true"></i>GitHub</a></li>
    {% endif %}
    {% if author.linkedin %}
      <li><a href="https://www.linkedin.com/in/{{ author.linkedin }}"><i class="fab fa-fw fa-linkedin icon-pad-right" aria-hidden="true"></i>LinkedIn</a></li>
    {% endif %}
    {% if author.mastodon %}
      <li><a href="{{ author.mastodon }}"><i class="fab fa-fw fa-mastodon icon-pad-right" aria-hidden="true"></i>Mastodon</a></li>
    {% endif %}
  </ul>
</div>
