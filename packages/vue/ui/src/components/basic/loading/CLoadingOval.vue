<template>
  <svg
    class="c-loading"
    stroke="currentColor"
    viewBox="0 0 38 38"
    xmlns="http://www.w3.org/2000/svg"
  >
    <g
      transform="translate(1 1)"
      stroke-width="2"
      fill="none"
      fill-rule="evenodd"
    >
      <circle stroke-opacity=".5" cx="18" cy="18" r="18"></circle>
      <path d="M36 18c0-9.94-8.06-18-18-18">
        <animateTransform
          attributeName="transform"
          type="rotate"
          from="0 18 18"
          to="360 18 18"
          dur="1s"
          repeatCount="indefinite"
        ></animateTransform>
      </path>
    </g>
  </svg>
</template>
