# sign-up-in-out

---

## Customize Authenticator

By adding company logo above main for and/or applying style to button (linear gradient).

[SRC: - Override and customize your Authenticator](https://ui.docs.amplify.aws/react/connected-components/authenticator/customization)

---

## Sign-out

Add sign-out button to default react app wrapped in aws authenticator:

```js
import logo from "./logo.svg";
import "./App.css";

import "@aws-amplify/ui-react/styles.css";
import { withAuthenticator } from "@aws-amplify/ui-react";
import { Auth } from "aws-amplify";

function App() {
  const handleSignOut = async () => {
    try {
      await Auth.signOut();
    } catch (error) {
      console.log("Error signing out:", error);
    }
  };

  return (
    <div className="App">
      <header className="App-header">
        <img src={logo} className="App-logo" alt="logo" />
        <p>
          Edit <code>src/App.js</code> and save to reload.
        </p>
        <a
          className="App-link"
          href="https://reactjs.org"
          target="_blank"
          rel="noopener noreferrer"
        >
          Learn React
        </a>
        <p style={{ color: "orange" }}>Scroll down to sign out..</p>
      </header>
      <button
        onClick={handleSignOut}
        style={{
          backgroundColor: "orange",
          width: "100%",
          padding: "10px", // Facultatif pour ajouter un peu d'espace autour du texte
          color: "white", // Facultatif pour changer la couleur du texte
          border: "none", // Facultatif pour supprimer la bordure
          cursor: "pointer", // Facultatif pour montrer qu'il s'agit d'un bouton
        }}
      >
        Sign Out
      </button>
    </div>
  );
}

export default withAuthenticator(App);
```

---
