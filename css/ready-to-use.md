<!-- Card Effect -->
```css
.card {
  /* Add shadows to create the "card" effect */
  box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2);
  transition: 0.3s;
  height: 70%;
  width: 45%;
}
/* On mouse-over, add a deeper shadow */
.card:hover {
  box-shadow: 0 8px 16px 0 rgba(0, 0, 0, 0.2);
}
```

/* Spinner effect */

```css

.spinner {
  animation: spinner infinite 0.9s linear;
  height: 90%;
}
.spinner:focus {
  border: none;
}
@keyframes spinner {
  from {
    transform: rotate(0deg);
  }
  to {
    transform: rotate(360deg);
  }
}

```


## Text effect fade in

```css
.text h1 {
  font-size: 40px;
  opacity: 0;
  animation: fadein 1s;
  animation-delay: 0.2s;
  animation-fill-mode: forwards;
}

@keyframes fadein {
  0% {
    opacity: 0;
  }
  100% {
    opacity: 1;
  }
}

```