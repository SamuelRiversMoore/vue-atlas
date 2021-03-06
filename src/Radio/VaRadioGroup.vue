<template>
  <div :class="classObj">
    <slot />
    <validate
      :name="name"
      :rules="rules"
      :custom-validate="customValidate"
      :current="value">
    </validate>
  </div>
</template>

<script>
import events from '../utils/events'
import type from '../utils/type'
import validate from '../validate'
import validationMixin from '../Mixin/validationMixin'

export default {
  name: 'VaRadioGroup',
  mixins: [validationMixin, events],
  props: {
    value: {
      type: [String, Array]
    },
    vertical: {
      type: Boolean,
      default: false
    },
    prefixCls: {
      type: String,
      default: 'va'
    }
  },
  data () {
    return {
      currentValue: ''
    }
  },
  methods: {
    init (val) {
      if (!type.isUndefined(val)) {
        this.currentValue = val
      } else {
        // first load
        let children = this.$children
        let ret

        children.forEach((item) => {
          item.currentChecked ? ret = item.label : ''
        })
        this.currentValue = ret
      }
    }
  },
  watch: {
    value (val) {
      this.currentValue = val
    },
    currentValue (val) {
      this.broadcast('VaRadio', 'Va@radiogroupChange', val)
      this.broadcast('VaRadioBtn', 'Va@radiogroupChange', val)
      this.$emit('input', val)
      this.$emit('change', val)
    }
  },
  created () {
    this.$on('Va@radioChange', (val) => {
      this.init(val)
    })
  },
  mounted () {
    this.currentValue = this.value
    this.$nextTick(() => {
      this.init()
    })
  },
  components: {
    validate
  },
  computed: {
    classObj () {
      let {prefixCls, vertical} = this
      let klass = {}

      // The -btn-group class is only necessary to give the proper border radius to buttons
      // in a radio group if radio buttons are used instead of regular radio elements.
      klass[prefixCls + '-btn-group'] = !vertical
      klass[prefixCls + '-btn-group-vertical'] = vertical
      klass[prefixCls + '-radio-group'] = true
      klass[prefixCls + '-radio-group-vertical'] = vertical

      return klass
    }
  }
}
</script>
