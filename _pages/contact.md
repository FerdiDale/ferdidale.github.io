---
layout: page
title: Contact
permalink: /contact/
---

<div class="contact-container">

  <p>If you'd like to reach out, you can send me an email directly:</p>

  <div class="contact-form">

    <label>
      Your email:
      <input type="email" id="userEmail" placeholder="you@example.com">
    </label>

    <label>
      Subject:
      <input type="text" id="subject" placeholder="Subject">
    </label>

    <label>
      Message:
      <textarea id="message" rows="6" placeholder="Write your message..."></textarea>
    </label>

    <a id="mailLink" href="#" class="send-button">Send Email</a>

  </div>
</div>

<script>
function updateMailLink() {
  const email = document.getElementById("userEmail").value;
  const subject = document.getElementById("subject").value;
  const message = document.getElementById("message").value;

  const body =
`From: ${email}

${message}`;

  const mailto = `mailto:mail@address.com?subject=${encodeURIComponent(subject)}&body=${encodeURIComponent(body)}`;

  document.getElementById("mailLink").href = mailto;
}

document.querySelectorAll("#userEmail, #subject, #message")
  .forEach(el => el.addEventListener("input", updateMailLink));
</script>