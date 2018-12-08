<template lang="pug">
div#app
  nav.navbar.navbar-expand-lg.navbar-dark.bg-dark.shadow-sm
    span.navbar-brand
      a(href="" style="color: white;") Casting pool generator
    button.navbar-toggler(type="button" data-toggle="collapse" data-target="#navbarSupportedContent" aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="Toggle navigation")
      span.navbar-toggler-icon
    div#navbarSupportedContent.collapse.navbar-collapse
      ul.navbar-nav.mr-auto
      ul.navbar-nav
        li.nav-item
          a.nav-link(href="") Home
  main.py-md-4.pl-md-5.pr-md-5(role="main")
    div.container-fluid
      h1 App
</template>

<script>
export default {
  data () {
    return {
      // ...
    }
  }
}
</script>
