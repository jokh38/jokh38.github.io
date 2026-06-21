---
layout: archive
title: "CV"
permalink: /cv/
author_profile: true
redirect_from:
  - /resume
---

{% include base_path %}

Work experience
======
* 2015 – present: Medical Physicist / Researcher
  * Proton Therapy Center, Samsung Medical Center, Seoul, Korea
  * Proton therapy physics: Monte Carlo simulation (FLUKA, TOPAS, Geant4),
    patient-specific QA for wobbling, line-scanning, and pencil-beam-scanning
    beam delivery, in-vivo range verification, and secondary neutron dosimetry.

Education
======
* Ph.D. in Physics (holographic QCD / AdS-CFT correspondence)
* Earlier training in theoretical high-energy physics (publications in
  JHEP and Physical Review D, 2008–2011).

Skills
======
* Monte Carlo simulation: FLUKA, TOPAS, Geant4
* Treatment planning: RayStation, XiO, Eclipse
* Patient-specific QA: machine log-file analysis, gamma analysis,
  2D/3D dosimetry
* Programming: Python, C++, MATLAB, deep learning (CNN-based dose analysis)
* Radiation dosimetry: photon (IMRT, VMAT, Tomotherapy), proton (PBS, line scanning, wobbling), HDR brachytherapy

Selected honors & infrastructure
======
* Contributing physicist to the **first private-hospital-based proton therapy
  center in Korea** (Samsung Medical Center, operational since 2015).

Publications
======
  <ul>{% for post in site.publications reversed %}
    {% include archive-single-cv.html %}
  {% endfor %}</ul>

Talks
======
  <ul>{% for post in site.talks reversed %}
    {% include archive-single-talk-cv.html  %}
  {% endfor %}</ul>

Teaching
======
  <ul>{% for post in site.teaching reversed %}
    {% include archive-single-cv.html %}
  {% endfor %}</ul>
