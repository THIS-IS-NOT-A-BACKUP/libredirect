<button {...$$restProps} on:click {...$$props}>
  <slot></slot>
</button>

<style>
  button {
    color: var(--text);
    border: none;
    text-decoration: none;
    cursor: pointer;
    font-size: 16px;
    font-weight: bold;
    transition-duration: 0.1s;
    display: inline-flex;
    align-items: center;
    margin: 7.5px 0;
    background-color: var(--bg-secondary);
    border-radius: 5px;
    padding: 10px;
  }

  button:hover:enabled {
    color: var(--active);
  }

  button:active:enabled {
    transform: translateY(1px);
  }

  button:disabled {
    cursor: not-allowed;
    opacity: 0.5;
  }
  
</style>
