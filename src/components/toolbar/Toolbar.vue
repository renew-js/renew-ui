<template>
  <div class="rnw-toolbar">
    <div class="rnw-toolbar-tools">
      <div
        v-for="(tool, i) in tools"
        :key="i"
      >
        <div
          v-if="tool.type === 'separator'"
          class="rnw-toolbar-separator"
        />
        <div
           v-else
          class="rnw-toolbar-tool"
          @click="tool.action"
          :class="{ 'disabled': tool.disabled }"
          @dblclick="tool.dblclickAction"
        >
          <i :class="tool.icon" />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { mapState } from 'vuex';


export default {
    name: 'Toolbar',
    data () {
        return {
            tools: [
                {
                    type: 'undo',
                    icon: 'icon-undo',
                    action: this.undoCommand,
                },
                {
                    type: 'redo',
                    icon: 'icon-redo',
                    action: this.redoCommand,
                },
                { type: 'separator' },
                {
                    type: 'bold',
                    icon: 'icon-bold',
                    dblclickAction : this.boldDblClickCommand,
                    action: this.boldCommand,
                    disabled: false,
                    
                },
                {
                    type: 'italic',
                    icon: 'icon-italic',
                    dblclickAction : this.italicDblClickCommand,
                    action: this.italicCommand,
                    disabled: false,
                },
                {
                    type: 'underline',
                    icon: 'icon-underline',
                    dblclickAction : this.underlineDblClickCommand,
                    action: this.underlineCommand,
                    disabled: false,
                },
                { type: 'separator' },
                {
                    type: 'align-left',
                    icon: 'icon-align-left',
                    dblclickAction : this.textAlignLeftDblClickCommand,
                    action: this.textAlignLeftCommand,
                    disabled: false,
                },
                {
                    type: 'align-center',
                    icon: 'icon-align-center',
                    dblclickAction : this.textAlignCenterDblClickCommand,
                    action: this.textAlignCenterCommand,
                    disabled: false,
                },
                {
                    type: 'align-right',
                    icon: 'icon-align-right',
                    dblclickAction : this.textAlignRightDblClickCommand,
                    action: this.textAlignRightCommand,
                    disabled: false,
                },
            ],
        };
    },
    
    
    computed: mapState([ 'activeContext' ]),
    methods: {
        undoCommand () {
            this.$instances[this.activeContext].fire('command.undo');
        },
        redoCommand () {
            this.$instances[this.activeContext].fire('command.redo');
        },
        italicCommand () {
          const attribute = 'italic';
          this.$instances[this.activeContext].fire('textstyle.fontstyle', attribute);
        },
        boldCommand () {
          const attribute = 'bold';
          this.$instances[this.activeContext].fire('textstyle.fontweight', attribute);
        },
        underlineCommand () {
          const attribute = 'underline';
          this.$instances[this.activeContext].fire('textstyle.textdecoration', attribute);
        },
        textAlignLeftCommand () {
        this.$instances[this.activeContext].fire('textstyle.textalignment', "left-middle");
        },
        textAlignCenterCommand () {
        this.$instances[this.activeContext].fire('textstyle.textalignment', "center-middle");
        },
        textAlignRightCommand () {
          this.$instances[this.activeContext].fire('textstyle.textalignment', "right-middle");
        },
        italicDblClickCommand () {
        this.$instances[this.activeContext].fire('edit.dblClickTextstyle',"font-style:italic");
        },
        boldDblClickCommand () {
        this.$instances[this.activeContext].fire('edit.dblClickTextstyle',"font-weight:bold");
        },
        underlineDblClickCommand () {
        this.$instances[this.activeContext].fire('edit.dblClickTextstyle',"text-decoration:underline");
        },
        textAlignCenterDblClickCommand () {
        this.$instances[this.activeContext].fire('edit.dblClickTextstyle',"align:center-middle");
        },
        textAlignRightDblClickCommand () {
        this.$instances[this.activeContext].fire('edit.dblClickTextstyle',"align:right-middle");
        },
        textAlignLeftDblClickCommand () {
        this.$instances[this.activeContext].fire('edit.dblClickTextstyle',"align:left-middle");
        },
    },
};
</script>

<style scoped>
  .rnw-toolbar {
    height: 32px;
    width: 100%;
    background-color: #fafafa;
    border-bottom: 1px solid #ccc;
  }

  .rnw-toolbar-tools {
    margin-left: 22px;
    display: flex;
    flex-direction: row;
  }

  .rnw-toolbar-tool {
    margin: 2px 1px;
    padding: 4px 2px;
    width: 28px;
    height: 28px;
    border-radius: 4px;
    color: #444;
    cursor: pointer;
  }

  .rnw-toolbar-tool:hover {
    background-color: #eee;
  }

  .rnw-toolbar-separator {
    border-left: 1px solid #ccc;
    margin: 7px 5px;
    height: 18px;
  }
  .disabled {
    pointer-events: none;
    color: #999;
  }
</style>
