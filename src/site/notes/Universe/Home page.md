---
{"dg-publish":true,"permalink":"/universe/home-page/","tags":["gardenEntry"]}
---

# Wiki
---
### Mondes:
Liste des Mondes répertoriés


<script>
  document.addEventListener("DOMContentLoaded", () => {
    document.querySelectorAll("h2").forEach(header => {
      const section = document.createElement("div");
      section.classList.add("section-content");

      let next = header.nextElementSibling;
      while (next && !next.matches("h1, h2")) {
        const temp = next.nextElementSibling;
        section.appendChild(next);
        next = temp;
      }

      header.after(section);
      header.classList.add("collapsible");

      header.addEventListener("click", () => {
        header.classList.toggle("collapsed");
        section.classList.toggle("hidden");
      });
    });
  });
</script>



## test
Text test.