<template>
  <div v-if="$themeConfig.vssue.need">
      <Vssue
        :title="$page.title"
        :options="options"
        class="vssue-warp"
      />
  </div>
</template>

<script>
import "imStyles/vssue.styl";
import { VssueComponent } from 'vssue'
import GithubV3 from '@vssue/api-github-v3'

export default {
  name: 'VssueDemo',
  components: {
    'Vssue': VssueComponent,
  },
  data () {
    return {
      title: 'Vssue Demo'
    }
  },
  computed: {
    options() {
      let name;
      if (process.env.NODE_ENV === "development") {
        name = "development";
      } else {
        name = "production";
      }
      return Object.assign(
        { api: GithubV3 },
        this.optionFilter(this.$themeConfig.vssue.option),
        this.optionFilter(this.$themeConfig.vssue[name])
      );
    }
  },
  methods: {
    optionFilter(obj) {
      if (Object.prototype.toString.call(obj).split(8, -1) !== "object") {
        return {};
      }
      for (const key in obj) {
        if (obj.hasOwnProperty(key) && !obj[key]) {
          delete obj[key];
        }
      }
      return obj;
    }
  }
}
</script>
<style lang="stylus">
.vssue-warp {
  box-shadow: 0 8px 10px -5px rgba(0, 0, 0, 0.2), 0 16px 24px 2px rgba(0, 0, 0, 0.14), 0 6px 30px 5px rgba(0, 0, 0, 0.12);
  background: white;
  border-radius: 10px;
  margin: 20px 0;
}
</style>
