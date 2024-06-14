# toastify

[React-toastify](https://fkhadra.github.io/react-toastify/introduction/)

---

# Position

To customize default position.

[ref: react-toastify: How to position the toasts a little up from the bottom](https://stackoverflow.com/questions/67651570/react-toastify-how-to-position-the-toasts-a-little-up-from-the-bottom)

app.css:

```css
.toast-position {
  bottom: 2rem !important;
}
```

index.tsx:

```tsx
...
import { ToastContainer, toast } from "react-toastify";
import "react-toastify/dist/ReactToastify.css";
...
const notifyCopiedToClipboard = () => {
  toast.success("Copied to clipboard!", {
    position: "bottom-right",
    autoClose: 1000, // 2 secondes
    hideProgressBar: false,
    closeOnClick: true,
    pauseOnHover: true,
    draggable: true,
    progress: undefined,
  });
};
...
<Tooltip
  title={"Copy to clipboard.."}
  arrow
  slotProps={{
    popper: {
      modifiers: [
        {
          name: "offset",
          options: {
            offset: [0, -14],
          },
        },
      ],
    },
  }}
>
  <IconButton
    style={{ marginRight: "8px" }}
    onClick={() =>
      handleCopyToClipboard(
        configuration.fileName || ""
      )
    }
  >
    <ContentCopyIcon />
    <ToastContainer
      className="toast-position"
      position="bottom-right"
      autoClose={10000}
      hideProgressBar={false}
      newestOnTop={false}
      closeOnClick
      rtl={false}
      pauseOnFocusLoss
      draggable
      pauseOnHover
      theme="light"
    />
  </IconButton>
</Tooltip>
...
```

---
