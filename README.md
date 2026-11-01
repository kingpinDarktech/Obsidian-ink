<script>
/* ---------- Mobile navigation ---------- */
const menuBtn = document.getElementById("menuBtn");
const navLinks = document.getElementById("navLinks");

menuBtn.addEventListener("click", () => {
  const open = navLinks.classList.toggle("open");
  menuBtn.setAttribute("aria-expanded", open);
  menuBtn.textContent = open ? "✕" : "☰";
});

document.querySelectorAll(".nav-links a").forEach(link => {
  link.addEventListener("click", () => {
    navLinks.classList.remove("open");
    menuBtn.setAttribute("aria-expanded", "false");
    menuBtn.textContent = "☰";
  });
});

/* ---------- Package buttons pre-select the package ---------- */
document.querySelectorAll("[data-package]").forEach(link => {
  link.addEventListener("click", () => {
    const wanted = link.dataset.package;
    const select = document.getElementById("package");
    let matched = false;

    Array.from(select.options).forEach(option => {
      const isMatch = option.text.trim() === wanted;
      option.selected = isMatch;
      if (isMatch) matched = true;
    });

    // Fallback if the option text ever changes
    if (!matched) {
      const fallback = Array.from(select.options)
        .find(o => o.text.trim() === "Not sure yet");
      if (fallback) fallback.selected = true;
    }
  });
});

/* ---------- Booking form -> mailto ---------- */
const bookingForm = document.getElementById("bookingForm");
const STUDIO_EMAIL = "obsidianinkstudio361@gmail.com";

bookingForm.addEventListener("submit", event => {
  event.preventDefault();

  if (!bookingForm.checkValidity()) {
    bookingForm.reportValidity();
    return;
  }

  const data = new FormData(bookingForm);
  const value = key => (data.get(key) || "").toString().trim();

  const name        = value("name");
  const email       = value("email");
  const phone       = value("phone") || "Not provided";
  const pkg         = value("package");
  const style       = value("style");
  const date        = value("date") || "Flexible";
  const placement   = value("placement");
  const size        = value("size") || "Not specified";
  const description = value("description");
  const reference   = bookingForm.querySelector("#reference").files.length > 0;

  const subject = `Tattoo Booking Request – ${pkg} – ${name}`;

  const body = [
    "NEW BOOKING REQUEST — OBSIDIAN INK",
    "-----------------------------------",
    `Full Name:        ${name}`,
    `Email:            ${email}`,
    `Phone:            ${phone}`,
    `Package:          ${pkg}`,
    `Tattoo Style:     ${style}`,
    `Preferred Date:   ${date}`,
    `Body Placement:   ${placement}`,
    `Approximate Size: ${size}`,
    "",
    "TATTOO IDEA",
    "-----------------------------------",
    description,
    "",
    "-----------------------------------",
    reference
      ? "Reference image selected — please attach it to this email manually."
      : "No reference image selected."
  ].join("\n");

  window.location.href =
    `mailto:${STUDIO_EMAIL}` +
    `?subject=${encodeURIComponent(subject)}` +
    `&body=${encodeURIComponent(body)}`;
});

/* ---------- Footer year ---------- */
document.getElementById("year").textContent = new Date().getFullYear();
</script>
