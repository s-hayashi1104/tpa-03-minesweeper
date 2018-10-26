<template>
    <td
    :class="state"
    @click.prevent="openThis"
    @click.right.prevent="setThis"
    >
    </td>
</template>

<script>
export default {
  name: 'TheTile',
  props: ['state'],
  method:{
    openThis:() => {
      this.$emit('openTile', this.state);
    },
    setThis:()=>{
      this.$emit('setFlag', this.state);
    },
  }
};
</script>

<style>
td.unopened {
  background: url('../assets/unopened.svg');
  cursor: pointer;
}

td.opened {
  background: url('../assets/opened.svg');
}

td.flagged {
  background: url('../assets/flag.svg');
  cursor: pointer;
}

td.mine {
  background: url('../assets/mine.png');
}

td.mine-neighbor-1 {
  background: url('../assets/1.svg');
}

td.mine-neighbor-2 {
  background: url('../assets/2.svg');
}

td.mine-neighbor-3 {
  background: url('../assets/3.svg');
}

td.mine-neighbor-4 {
  background: url('../assets/4.svg');
}

td.mine-neighbor-5 {
  background: url('../assets/5.svg');
}

td.mine-neighbor-6 {
  background: url('../assets/6.svg');
}

td.mine-neighbor-7 {
  background: url('../assets/7.svg');
}

td.mine-neighbor-8 {
  background: url('../assets/8.svg');
}
</style>
