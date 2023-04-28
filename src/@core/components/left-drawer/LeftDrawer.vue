<template>
  <VNavigationDrawer
    :rail="rail"
    color="drawer"
    elevation="7"
    expand-on-hover
    rail-width="55"
    width="260"
  >
    <VList nav>
      <VListItem
        :ripple="false"
        active-color="primary"
        prepend-icon="$mdiHomeOutline"
        title="Assignment"
        to="/"
      />
      <VListItem
        :ripple="false"
        active-color="primary"
        prepend-icon="$mdiHomeOutline"
        title="Generate XML"
        to="/generator"
      />
    </VList>
  </VNavigationDrawer>
</template>
