---
layout: page
title: Send me an e-mail
permalink: /contact/
---

<div class="contact-container">

  <p>Feel free to reach out, I would be thrilled to hear from you!</p>

  <div class="contact-form">

    <label>
      Subject:
      <input type="text" id="subject" placeholder="Subject">
    </label>

    <label>
      Message:
      <textarea id="message" rows="6" placeholder="Write your message..."></textarea>
    </label>

    <a id="mailLink" href="#" class="send-button">Send</a>

  </div>
</div>

<script>
function updateMailLink() {
  const subject = document.getElementById("subject").value;
  const message = document.getElementById("message").value;

  const mailto = `mailto:f.dalessandro.02@gmail.com?subject=${encodeURIComponent(subject)}&body=${encodeURIComponent(message)}`;

  document.getElementById("mailLink").href = mailto;
}

document.querySelectorAll("#userEmail, #subject, #message")
  .forEach(el => el.addEventListener("input", updateMailLink));
</script>