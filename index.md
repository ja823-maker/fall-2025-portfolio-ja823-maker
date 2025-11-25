---
layout: home
title: "John Apessos"
subtitle: "Mechanical Engineering @ Cornell University"
---

<div style="display:flex; gap:2rem; flex-wrap:wrap; align-items:flex-start;">

  <!-- TEXT COLUMN -->
  <div style="flex:2; min-width:260px;">
    <h1>Hi, I'm John</h1>

    <p>
      I'm a Mechanical Engineering student at Cornell (Class of 2027). I work on autonomous drones and
      high-performance mechanical systems, and this portfolio highlights my MAE projects and experience.
    </p>

    <p>
      <a href="{{ '/assets/John_resume_current.pdf' | relative_url }}" class="btn btn-primary">📄 Download my resume</a>
      <a href="{{ '/cv/' | relative_url }}" class="btn">View CV page</a>
    </p>

    <h2>Highlights</h2>
    <ul>
      <li>President & Full Team Technical Lead — Cornell University Autonomous Drone</li>
      <li>Mechanical design, CAD, and fabrication</li>
      <li>Controls, system dynamics, and mechatronics coursework</li>
    </ul>
  </div>

  <!-- IMAGE COLUMN (fixed: prevents horizontal collapse/cropping) -->
  <!-- IMAGE COLUMN (final working version) -->
<!-- IMAGE COLUMN (final-correct with top/bottom crop + rounded corners) -->
<div style="
  flex:0 0 auto;
  display:flex;
  justify-content:center;
  align-items:flex-start;
  max-width:420px;
">
  <div style="
    width:100%;
    max-height:500px;         /* vertical crop window */
    overflow:hidden;
    border-radius:16px;        /* now the rounded edges work! */
    box-shadow:0 8px 20px rgba(0,0,0,0.15);
  ">
    <img
      src="{{ '/assets/images/CUAD_John.JPG' | relative_url }}"
      alt="John Apessos CUAD Photo"
      style="
        width:100%;
        height:auto;
        display:block;
        object-fit:cover;
        object-position: center 10%;   /* crop more from top, less from bottom */
      "
    >
  </div>
</div>

</div>
