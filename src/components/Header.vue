<template>
  <div>
    <header class="header">
      <b-navbar toggleable="lg" type="info">
        <b-navbar-brand href="#">
          <b-icon-search></b-icon-search>Domain Search
        </b-navbar-brand>
        <b-navbar-toggle target="nav-collapse"></b-navbar-toggle>

        <b-collapse id="nav-collapse" is-nav>
          <b-navbar-nav></b-navbar-nav>

          <!-- Right aligned nav items -->
          <b-navbar-nav class="ml-auto"></b-navbar-nav>
        </b-collapse>
      </b-navbar>
    </header>
  </div>
</template>