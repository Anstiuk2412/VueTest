<template>
  <div class="marquee">
    <div class="marquee-content" :style="marquee">
      <span v-for="i in 4" :key="i" class="marquee-item">
        <a href="YOUR_LINK_HERE">{{ $t('KAEMPE FØDSELSDAGSUDSALG - SPAR HELT OP TIL 70% - FÅ GRATIS SENGESÆT MED TIL ALLE SENGE - BEGRÆNSET ANTAL - KØB NU')}}</a>
        </span>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Marquee',
  mounted() {
    window.addEventListener('scroll', this.handleScroll);
  },
  beforeDestroy() {
    window.removeEventListener('scroll', this.handleScroll);
  },
  data() {
    return {
      marquee: {
        width: '100%'
      },
    };
  },
  methods: {
    handleScroll(e) {
      const scrollTop = window.pageYOffset || document.documentElement.scrollTop;
      if (scrollTop > 30) {
        this.marquee.width = '50%';
      } else {
        this.marquee.width = '100%';
      }
    },
  },
}
</script>

<style scoped>
.marquee {
  overflow: hidden;
  position: relative;
  white-space: nowrap;
}

.marquee-content {
  display: inline-block;
 /* width: 100%;*/
  animation: marquee 25s linear infinite;
}

.marquee-item {
  display: inline-block;
  width: unset;
  a{
    padding: 5px 0 5px 0;
    color: white;
    font-size: 16px;
  }
  a:hover{
    text-decoration: underline;
  }
}

@keyframes marquee {
  0% { transform: translate(0); }
  100% { transform: translate(-100%); }
}
</style>
