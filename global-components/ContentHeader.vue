<template>
  <div class="content-header index-header">
    <div class="container fade-scale in">
      <h1 id="conentHeader" class="title" :class="{'post-content-header': isPosts}">{{title}}</h1>
      <h5 class="subtitle">{{description}}</h5>
    </div>
  </div>
</template>
<script>
  import Bus from "imUtils/Bus";
  export default {
    name: "FixedHeader",
    data() {
      return {
        description: "",
        isPosts: false,
        tagName: ""
      };
    },
    props: {
      content: {
        type: Array,
        default: () => []
      }
    },
    computed: {
      title() {
        let t;
        switch (this.$route.path.slice(1, 6)) {
          case "posts":
            t = this.$page.title;
            this.isPosts = true;
            this.description = this.$page.lastUpdated
              ? "最后更新时间：" + this.$page.lastUpdated
              : "";
            break;
          case "all/":
            t = this.$themeConfig.menus.all || "时间归档";
            this.isPosts = false;
            if (this.content.length === 0) {
              return (this.description = "");
            }
            setTimeout(() => {
              this.description =
                this.content[this.content.length - 1].lastUpdated.slice(0, 7) +
                "~" +
                this.content[0].lastUpdated.slice(0, 7) +
                " ===>>> 共" +
                this.content.length +
                "篇";
            }, 20);
            break;
          case "tags/":
            t = this.tagName || "";
            this.isPosts = false;
            this.description = "";
            break;
          case "about":
            t = this.$themeConfig.menus.about || "自我介绍";
            this.isPosts = false;
            this.description = "";
            break;
          default:
            t = this.$site.title || "欢迎光临";
            this.isPosts = false;
            this.description = this.$site.description || "期待与你的交流";
        }
        return t;
      }
    },
    mounted() {
      Bus.$on("changeContentHeader", tagName => {
        this.tagName = tagName;
      });
    }
  };
</script>
<style lang="scss" scoped>
  .content-header {
    padding: 49px 16px 45px 354px;
    margin-left: -249px;
    width: 100%;
    color: #fff;
    background: #3f51b5;
    text-shadow: 0 1px 1px rgba(0, 0, 0, 0.2);
  }

  .content-header .subtitle {
    padding-top: 11px;
    font-weight: 300;
    color: #c5cae9;
    font-size: 18px;
    line-height: 24px;
  }

  .content-header .title {
    font-weight: 500;
    font-size: 36px;
    line-height: 48px;
  }
  .post-content-header {
    font-weight: 400;
    font-size: 33px;
    line-height: 48px;
  }
  @media (max-width: 1190px) {
    .content-header {
      margin-left: 0;
      padding-left: 21px;
      width: auto;
    }
    .content-header .title {
      font-size: 28px;
    }
  }
</style>