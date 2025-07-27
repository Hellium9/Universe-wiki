---
{"dg-publish":true,"permalink":"/universe/home-page/","tags":["gardenEntry"]}
---

# Wiki
---
### Mondes:
Liste des Mondes répertoriés: 




 Mondes Répertoriés 

<div class="transclusion internal-embed is-loaded"><div class="markdown-embed">



# Mondes
```dvjs
const TAG = "World";
const pages = dv.pages().filter(p => p.tags && p.tags.includes(TAG));

let nodes = new Map();

// Étape 1 : Créer les nœuds
for (let page of pages) {
    const name = page.file.name;
    let parent = null;

    if (page.parent) {
        if (typeof page.parent === "object" && page.parent.path) {
            // ✅ On extrait le nom du fichier parent, sans le chemin
            parent = page.parent.path.split("/").pop().replace(/\.md$/, "");
        } else if (typeof page.parent === "string") {
            parent = page.parent.replace(/\[\[|\]\]/g, "").trim();
        }
    }

    if (!nodes.has(name)) {
        nodes.set(name, { name: name, page: page, parent: parent, children: [] });
    } else {
        nodes.get(name).page = page;
        nodes.get(name).parent = parent;
    }

    // Crée un nœud vide pour le parent s'il n'existe pas encore
    if (parent && !nodes.has(parent)) {
        nodes.set(parent, { name: parent, page: dv.page(parent), parent: null, children: [] });
    }
}

// Étape 2 : Relier parents et enfants
for (let node of nodes.values()) {
    if (node.parent && nodes.has(node.parent)) {
        nodes.get(node.parent).children.push(node);
    }
}

// Étape 3 : Trouver les racines (ceux qui ne sont enfants de personne)
const allChildren = new Set();
for (let node of nodes.values()) {
    for (let child of node.children) {
        allChildren.add(child.name);
    }
}
const roots = [...nodes.values()]
    .filter(n => !allChildren.has(n.name) && n.page)
    .sort((a, b) => a.name.localeCompare(b.name));

// Fonction pour créer un lien Obsidian cliquable
function makeLink(name) {
    const file = app.metadataCache.getFirstLinkpathDest(name, "");
    if (!file) return name;

    const link = document.createElement("a");
    link.href = file.path;
    link.innerText = name;
    link.dataset.href = file.path;
    link.classList.add("internal-link");
    return link;
}

// Rendu récursif
function renderNode(node, parentEl) {
    const li = parentEl.createEl("li");
    li.append(makeLink(node.name));

    if (node.children.length > 0) {
        const ul = li.createEl("ul");
        const sorted = node.children.sort((a, b) => a.name.localeCompare(b.name));
        for (let child of sorted) {
            renderNode(child, ul);
        }
    }
}

// Affichage
const ul = dv.el("ul", "");
for (let root of roots) {
    renderNode(root, ul);
}
```

</div></div>






