# 124-articles-list

Design setup on how to render article list to home page.

---

## index.html

```html
...
<div class="content">
  <div class="articles-container">
    <div class="article">
      <img
        src="https://randomuser.me/api/portraits/men/73.jpg"
        alt="profile"
      />
      <h2>Article title</h2>
      <p class="article-author">John Doe</p>
      <p class="article-content">
        Napoléon Ier, né le 15 août 1769 à Ajaccio et mort le 5 mai 1821
        sur l'île Sainte-Hélène, est le premier empereur des Français, du
        18 mai 1804 au 6 avril 1814 et du 20 mars 1815 au 22 juin 1815.
        Second enfant de Charles Bonaparte et Letizia Ramolino, Napoléon
        Bonaparte est un militaire, général dans les armées de la Première
        République française, née de la Révolution, commandant en chef de
        l'armée d'Italie puis de l'armée d'Orient. Arrivé au pouvoir en
        1799 par le coup d'État du 18 Brumaire, il est Premier consul
        jusqu'au 2 août 1802, puis consul à vie jusqu'au 18 mai 1804, date
        à laquelle il est proclamé empereur des Français par un
        sénatus-consulte suivi d'un plébiscite. Il est sacré empereur, en
        la cathédrale Notre-Dame de Paris, le 2 décembre 1804, par le pape
        Pie VII. Son épouse, l'impératrice Joséphine de Beauharnais, est
        également sacrée. En tant que général en chef et chef d'État,
        Napoléon tente de briser les coalitions montées et financées par
        le royaume de Grande-Bretagne et qui rassemblent, à partir de
        1792, les monarchies européennes contre la France et son régime né
        de la Révolution. Il conduit les armées françaises d'Italie au Nil
        et d'Autriche à la Prusse et à la Pologne : les nombreuses et
        brillantes victoires de Bonaparte (Arcole, Rivoli, Pyramides,
        Marengo, Austerlitz, Iéna, Friedland), dans des campagnes
        militaires rapides, disloquent les quatre premières coalitions.
        Les paix successives, qui mettent un terme à chacune de ces
        coalitions, renforcent la France et donnent à Napoléon un degré de
        puissance jusqu'alors rarement égalé en Europe, lors de la paix de
        Tilsit (1807).
      </p>
      <div class="article-actions">
        <button class="btn btn-danger">Supprimer</button>
        <button class="btn btn-primary">Modifier</button>
      </div>
    </div>
  </div>
</div>
...
```

---

## index.scss

```scss
.content {
  display: flex;
  justify-content: center;
  align-items: center;
  .articles-container {
    max-width: 800px;
    width: 100%;
    margin: 5rem 0 10rem 0;
    .article {
      background: white;
      padding: 0 5rem;
      border-radius: 0.3rem;
      display: flex;
      margin-bottom: 5rem;
      flex-direction: column;
      align-items: center;
      box-shadow: var(--box-shadow);
      img {
        height: 6rem;
        width: 6rem;
        border-radius: 50%;
        margin-top: -3rem;
      }
      h2 {
        margin-top: 2rem;
        margin-bottom: 0;
      }
      .article-author {
        color: var(--hint);
        margin-bottom: 3rem;
      }
      .article-content {
        max-width: 550px;
      }
      .article-actions {
        width: 100%;
        display: flex;
        justify-content: flex-end;
        padding: 2rem 0;
        margin-top: 2rem;
        border-top: 1px solid var(--divider);
        .btn {
          margin-left: 1rem;
        }
      }
    }
  }
}
```

---

## _variables.scss

```scss
...
--hint: #95a5a6;
...
```

---

## styles.scss

```scss
...
.content {
  ...
  background: var(--divider);
}
...
```

---

## form.scss

```scss
.content {
  ...
  form {
    background: white;
    ...
  }
  ...
}
...
```

---
