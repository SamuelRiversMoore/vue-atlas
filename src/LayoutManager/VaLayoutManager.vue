<template>
  <div :style="styleObj">
    <slot/>
  </div>
</template>

<script>
import events from '../utils/events'

export default {
  name: 'VaLayoutManager',
  mixins: [events],
  props: {
    desktopSidebarWidth: {
      type: [Number, String],
      default: 0,
      required: false
    },
    desktopMinibarWidth: {
      type: [Number, String],
      default: 0,
      required: false
    },
    desktopTopbarHeight: {
      type: [Number, String],
      default: 0,
      required: false
    },
    mobileFriendly: {
      type: Boolean,
      default: true
    },
    mobileBreakpoint: {
      type: Number,
      default: 768,
      required: false
    },
    mobileSidebarWidth: {
      type: [Number, String],
      default: 0,
      required: false
    },
    mobileMinibarWidth: {
      type: [Number, String],
      default: 0,
      required: false
    },
    mobileTopbarHeight: {
      type: [Number, String],
      default: 0,
      required: false
    },
    rtl: {
      type: Boolean,
      default: false
    },
    sidebarPriority: {
      type: Boolean,
      default: false,
      required: false
    },
    minibarPriority: {
      type: Boolean,
      default: false,
      required: false
    },
    topbarPriority: {
      type: Boolean,
      default: false,
      required: false
    },
    topbarPadded: {
      type: Boolean,
      default: false,
      required: false
    },
    split: {
      type: Boolean,
      default: false,
      required: false
    },
    reverse: {
      type: Boolean,
      default: false,
      required: false
    },
    desktopMargin: {
      type: [Number, String],
      default: 0,
      required: false
    },
    desktopMinimumWidth: {
      type: [Number, String],
      default: 0,
      required: false
    },
    bgColor: {
      type: String,
      default: '#F4F5F7',
      required: false
    },
    pageBgColor: {
      type: String,
      default: '#FFFFFF',
      required: false
    },
    prefixCls: {
      type: String,
      default: 'va'
    }
  },
  data () {
    let dDesktopMinimumWidth = this.desktopMinimumWidth
    let dDesktopTopbarHeight = this.desktopTopbarHeight
    let dDesktopMinibarWidth = this.desktopMinibarWidth
    let dDesktopSidebarWidth = this.desktopSidebarWidth
    let dMobileSidebarWidth = this.mobileSidebarWidth
    let dMobileMinibarWidth = this.mobileMinibarWidth
    let dMobileTopbarHeight = this.mobileTopbarHeight
    let dSidebarPriority = this.sidebarPriority
    let dMinibarPriority = this.minibarPriority
    let currDesktopMargin = this.desktopMargin
    let dTopbarPriority = this.topbarPriority
    let dDesktopMargin = this.desktopMargin
    let dTopbarPadded = this.topbarPadded
    let dPageBgColor = this.pageBgColor
    let dReverse = this.reverse
    let dBgColor = this.bgColor
    let dSplit = this.split
    let dRtl = this.rtl
    return {
      mDesktopTopbarHeight: dDesktopTopbarHeight,
      mDesktopMinimumWidth: dDesktopMinimumWidth,
      mDesktopSidebarWidth: dDesktopSidebarWidth,
      mDesktopMinibarWidth: dDesktopMinibarWidth,
      mMobileSidebarWidth: dMobileSidebarWidth,
      mMobileMinibarWidth: dMobileMinibarWidth,
      mMobileTopbarHeight: dMobileTopbarHeight,
      currentDesktopMargin: currDesktopMargin,
      mMinibarPriority: dMinibarPriority,
      mSidebarPriority: dSidebarPriority,
      mTopbarPriority: dTopbarPriority,
      mDesktopMargin: dDesktopMargin,
      mTopbarPadded: dTopbarPadded,
      pastMobileBreakpoint: false,
      mPageBgColor: dPageBgColor,
      allowMarginUpdates: true,
      currentMinibarWidth: 0,
      currentTopbarHeight: 0,
      currentSidebarWidth: 0,
      mMinibarTheme: 'blue',
      mSidebarTheme: 'blue',
      mTopbarTheme: 'blue',
      mReverse: dReverse,
      haveMinibar: false,
      mShowToggle: false,
      mBgColor: dBgColor,
      haveSidebar: false,
      haveTopbar: false,
      mTextLinks: false,
      mCompact: false,
      havePage: false,
      isMobile: false,
      contentWidth: 0,
      mPageSize: 'md',
      mSplit: dSplit,
      windowWidth: 0,
      mRtl: dRtl
    }
  },
  watch: {
    pastMobileBreakpoint (val) {
      this.broadcastIsMobile(val)
      this.setAndBroadcastDimensions()
    },
    mDesktopSidebarWidth (val) {
      this.setAndBroadcastDimensions()
    },
    mDesktopMinibarWidth (val) {
      this.setAndBroadcastDimensions()
    },
    mDesktopTopbarHeight (val) {
      this.setAndBroadcastDimensions()
    },
    mMobileSidebarWidth (val) {
      this.setAndBroadcastDimensions()
    },
    mMobileMinibarWidth (val) {
      this.setAndBroadcastDimensions()
    },
    mMobileTopbarHeight (val) {
      this.setAndBroadcastDimensions()
    },
    mRtl (val) {
      this.broadcastIsRTL(val)
    },
    mSidebarPriority (val) {
      this.broadcastSidebarPriority(val)
    },
    mMinibarPriority (val) {
      this.broadcastMinibarPriority(val)
    },
    mTopbarPriority (val) {
      this.broadcastTopbarPriority(val)
    },
    mTopbarPadded (val) {
      this.broadcastTopbarPadded(val)
    },
    mBgColor (val) {
      this.broadcastBgColor(val)
    },
    mPageBgColor (val) {
      this.broadcastPageBgColor(val)
    },
    mSplit (val) {
      this.broadcastIsSplit(val)
    },
    mReverse (val) {
      this.broadcastIsReverse(val)
    },
    mCompact (val) {
      this.broadcastCompact(val)
    },
    mTextLinks (val) {
      this.broadcastTextLinks(val)
    },
    mShowToggle (val) {
      this.broadcastShowToggle(val)
    },
    mDesktopMargin (val) {
      /**
       * Calling _handleResize here. Why? So a new contentWidth
       * is calculated and broadcasted.
       */
      this._handleResize()
      this.broadcastDesktopMargin(val)
    },
    mDesktopMinimumWidth (val) {
      this.broadcastDesktopMinimumWidth(val)
    },
    windowWidth (val) {
      this.broadcastWindowWidth(val)
    },
    contentWidth (val) {
      this.broadcastContentWidth(val)
    },
    allowMarginUpdates (val) {
      this.broadcastAllowMarginUpdates(val)
    },
    mPageSize (val) {
      this.broadcastPageSize(val)
    },
    mTopbarTheme (val) {
      this.broadcastTopbarTheme(val)
    },
    mMinibarTheme (val) {
      this.broadcastMinibarTheme(val)
    },
    mSidebarTheme (val) {
      this.broadcastSidebarTheme(val)
    }
  },
  methods: {
    _handleResize () {
      let ww = window.innerWidth || document.body.clientWidth

      if (parseInt(ww) < this.mobileBreakpoint) {
        this.pastMobileBreakpoint = true
      } else {
        this.pastMobileBreakpoint ? this.pastMobileBreakpoint = false : true
      }

      this.windowWidth = parseInt(ww)
      this.contentWidth = this.windowWidth - (this.mDesktopMargin * 2)
    },
    broadcastSidebarWidth (val) {
      this.broadcast('VaSidebar', 'Va@sidebarWidthChange', val)
      this.broadcast('VaMinibar', 'Va@sidebarWidthChange', val)
      this.broadcast('VaTopbar', 'Va@sidebarWidthChange', val)
      this.broadcast('VaPage', 'Va@sidebarWidthChange', val)
    },
    broadcastMinibarWidth (val) {
      this.broadcast('VaSidebar', 'Va@minibarWidthChange', val)
      this.broadcast('VaMinibar', 'Va@minibarWidthChange', val)
      this.broadcast('VaTopbar', 'Va@minibarWidthChange', val)
      this.broadcast('VaPage', 'Va@minibarWidthChange', val)
    },
    broadcastTopbarHeight (val) {
      this.broadcast('VaSidebar', 'Va@topbarHeightChange', val)
      this.broadcast('VaMinibar', 'Va@topbarHeightChange', val)
      this.broadcast('VaTopbar', 'Va@topbarHeightChange', val)
      this.broadcast('VaPage', 'Va@topbarHeightChange', val)
    },
    broadcastIsMobile (val) {
      this.broadcast('VaSidebar', 'Va@sidebarIsMobile', val)
      this.broadcast('VaMinibar', 'Va@minibarIsMobile', val)
      this.broadcast('VaTopbar', 'Va@topbarIsMobile', val)
      this.broadcast('VaPage', 'Va@pageIsMobile', val)
      this.broadcast('VaRange', 'Va@rangeIsMobile', val)
      this.broadcast('VaMobile', 'Va@mobileIsMobile', val)
      this.broadcast('VaDesktop', 'Va@desktopIsMobile', val)
    },
    broadcastIsRTL (val) {
      this.broadcast('VaSidebar', 'Va@rtlChange', val)
      this.broadcast('VaMinibar', 'Va@rtlChange', val)
      this.broadcast('VaTopbar', 'Va@rtlChange', val)
      this.broadcast('VaPage', 'Va@rtlChange', val)
    },
    broadcastIsSplit (val) {
      this.broadcast('VaSidebar', 'Va@splitChange', val)
      this.broadcast('VaMinibar', 'Va@splitChange', val)
      this.broadcast('VaTopbar', 'Va@splitChange', val)
      this.broadcast('VaPage', 'Va@splitChange', val)
    },
    broadcastCompact (val) {
      this.broadcast('VaSidebar', 'Va@compactChange', val)
    },
    broadcastTextLinks (val) {
      this.broadcast('VaSidebar', 'Va@textLinksChange', val)
    },
    broadcastShowToggle (val) {
      this.broadcast('VaSidebar', 'Va@showToggleChange', val)
      this.broadcast('VaSidebarGroupItem', 'Va@showToggleChange', val)
      this.broadcast('VaSidebarGroupLevel', 'Va@showToggleChange', val)
    },
    broadcastBgColor (val) {
      /**
       * Nobody needs to know about BgColor.
       * Only relevant to this component.
       */
    },
    broadcastPageBgColor (val) {
      this.broadcast('VaPage', 'Va@pageBgColorChange', val)
    },
    broadcastIsReverse (val) {
      this.broadcast('VaSidebar', 'Va@reverseChange', val)
      this.broadcast('VaMinibar', 'Va@reverseChange', val)
      this.broadcast('VaTopbar', 'Va@reverseChange', val)
      this.broadcast('VaPage', 'Va@reverseChange', val)
    },
    broadcastSidebarPriority (val) {
      this.broadcast('VaSidebar', 'Va@sidebarPriorityChange', val)
      this.broadcast('VaMinibar', 'Va@sidebarPriorityChange', val)
      this.broadcast('VaTopbar', 'Va@sidebarPriorityChange', val)
    },
    broadcastMinibarPriority (val) {
      this.broadcast('VaSidebar', 'Va@minibarPriorityChange', val)
      this.broadcast('VaMinibar', 'Va@minibarPriorityChange', val)
      this.broadcast('VaTopbar', 'Va@minibarPriorityChange', val)
    },
    broadcastTopbarPriority (val) {
      this.broadcast('VaSidebar', 'Va@topbarPriorityChange', val)
      this.broadcast('VaMinibar', 'Va@topbarPriorityChange', val)
      this.broadcast('VaTopbar', 'Va@topbarPriorityChange', val)
    },
    broadcastTopbarPadded (val) {
      this.broadcast('VaTopbar', 'Va@topbarPaddedChange', val)
    },
    broadcastDesktopMargin (val) {
      this.broadcast('VaSidebar', 'Va@desktopMarginChange', val)
      this.broadcast('VaMinibar', 'Va@desktopMarginChange', val)
      this.broadcast('VaTopbar', 'Va@desktopMarginChange', val)
      this.broadcast('VaPage', 'Va@desktopMarginChange', val)
    },
    broadcastDesktopMinimumWidth (val) {
      this.broadcast('VaSidebar', 'Va@desktopMinimumWidthChange', val)
      this.broadcast('VaMinibar', 'Va@desktopMinimumWidthChange', val)
      this.broadcast('VaTopbar', 'Va@desktopMinimumWidthChange', val)
      this.broadcast('VaPage', 'Va@desktopMinimumWidthChange', val)
    },
    broadcastAllowMarginUpdates (val) {
    },
    broadcastWindowWidth (val) {
      this.broadcast('VaSidebar', 'Va@windowWidthChange', val)
      this.broadcast('VaMinibar', 'Va@windowWidthChange', val)
      this.broadcast('VaTopbar', 'Va@windowWidthChange', val)
      this.broadcast('VaPage', 'Va@windowWidthChange', val)
    },
    broadcastContentWidth (val) {
      this.broadcast('VaSidebar', 'Va@contentWidthChange', val)
      this.broadcast('VaMinibar', 'Va@contentWidthChange', val)
      this.broadcast('VaTopbar', 'Va@contentWidthChange', val)
      this.broadcast('VaPage', 'Va@contentWidthChange', val)
    },
    broadcastPageSize (val) {
      this.broadcast('VaPage', 'Va@pageSizeChange', val)
    },
    broadcastTopbarTheme (val) {
      this.broadcast('VaTopbar', 'Va@topbarThemeChange', val)
    },
    broadcastSidebarTheme (val) {
      this.broadcast('VaSidebar', 'Va@sidebarThemeChange', val)
    },
    broadcastMinibarTheme (val) {
      this.broadcast('VaMinibar', 'Va@minibarThemeChange', val)
    },
    checkForPresenceOfTopbar () {
      this.broadcast('VaTopbar', 'Va@topbarPresenceCheck', true)
    },
    checkForPresenceOfSidebar () {
      this.broadcast('VaSidebar', 'Va@sidebarPresenceCheck', true)
    },
    checkForPresenceOfMinibar () {
      this.broadcast('VaMinibar', 'Va@minibarPresenceCheck', true)
    },
    checkForPresenceOfPage () {
      this.broadcast('VaPage', 'Va@pagePresenceCheck', true)
    },
    setAndBroadcastDimensions () {
      if (this.pastMobileBreakpoint) {
        this.currentTopbarHeight = this.mMobileTopbarHeight
        this.currentMinibarWidth = this.mMobileMinibarWidth
        this.currentSidebarWidth = this.mMobileSidebarWidth
      } else {
        this.currentTopbarHeight = this.mDesktopTopbarHeight
        this.currentMinibarWidth = this.mDesktopMinibarWidth
        this.currentSidebarWidth = this.mDesktopSidebarWidth
      }
      this.broadcastSidebarWidth(this.currentSidebarWidth)
      this.broadcastMinibarWidth(this.currentMinibarWidth)
      this.broadcastTopbarHeight(this.currentTopbarHeight)
    },
    enableReceivers () {
      /**
       * These exist for VaLayoutManagerConfig.
       *
       * Setting these values should trigger the 'watch' on each of them,
       * which in turn calls the value's respective broadcast function,
       * broadcasting the new value to whatever has implemented a
       * LayoutManager.
       */
      this.$on('Va@configDesktopTopbarHeightChange', (val) => { this.mDesktopTopbarHeight = val })
      this.$on('Va@configDesktopMinibarWidthChange', (val) => { this.mDesktopMinibarWidth = val })
      this.$on('Va@configDesktopMinimumWidthChange', (val) => { this.mDesktopMinimumWidth = val })
      this.$on('Va@configDesktopSidebarWidthChange', (val) => { this.mDesktopSidebarWidth = val })
      this.$on('Va@configMobileTopbarHeightChange', (val) => { this.mMobileTopbarHeight = val })
      this.$on('Va@configMobileSidebarWidth', (val) => { this.mMobileSidebarWidth = val })
      this.$on('Va@configMobileMinibarWidth', (val) => { this.mMobileMinibarWidth = val })
      this.$on('Va@configSidebarPriorityChange', (val) => { this.mSidebarPriority = val })
      this.$on('Va@configMinibarPriorityChange', (val) => { this.mMinibarPriority = val })
      this.$on('Va@configTopbarPriorityChange', (val) => { this.mTopbarPriority = val })
      this.$on('Va@configDesktopMarginChange', (val) => { this.mDesktopMargin = val })
      this.$on('Va@configTopbarPaddedChange', (val) => { this.mTopbarPadded = val })
      this.$on('Va@configMinibarThemeChange', (val) => { this.mMinibarTheme = val })
      this.$on('Va@configSidebarThemeChange', (val) => { this.mSidebarTheme = val })
      this.$on('Va@configTopbarThemeChange', (val) => { this.mTopbarTheme = val })
      this.$on('Va@configPageBgColorChange', (val) => { this.mPageBgColor = val })
      this.$on('Va@configShowToggleChange', (val) => { this.mShowToggle = val })
      this.$on('Va@configPageSizeChange', (val) => { this.mPageSize = val })
      this.$on('Va@configCompactChange', (val) => { this.mCompact = val })
      this.$on('Va@configReverseChange', (val) => { this.mReverse = val })
      this.$on('Va@configBgColorChange', (val) => { this.mBgColor = val })
      this.$on('Va@configSplitChange', (val) => { this.mSplit = val })
      this.$on('Va@configRtlChange', (val) => { this.mRtl = val })
      this.$on('Va@configTextLinksChange', (val) => { this.mTextLinks = val })
    },
    broadcastDefaultsToConfig () {
      setTimeout(() => {
        this.broadcast('VaLayoutManagerConfig', 'Va@configReceiveDesktopTopbarHeight', this.mDesktopTopbarHeight)
        this.broadcast('VaLayoutManagerConfig', 'Va@configReceiveDesktopMinibarWidth', this.mDesktopMinibarWidth)
        this.broadcast('VaLayoutManagerConfig', 'Va@configReceiveDesktopMinimumWidth', this.mDesktopMinimumWidth)
        this.broadcast('VaLayoutManagerConfig', 'Va@configReceiveDesktopSidebarWidth', this.mDesktopSidebarWidth)
        this.broadcast('VaLayoutManagerConfig', 'Va@configReceiveMobileSidebarWidth', this.mMobileSidebarWidth)
        this.broadcast('VaLayoutManagerConfig', 'Va@configReceiveMobileMinibarWidth', this.mMobileMinibarWidth)
        this.broadcast('VaLayoutManagerConfig', 'Va@configReceiveMobileTopbarHeight', this.mMobileTopbarHeight)
        this.broadcast('VaLayoutManagerConfig', 'Va@configReceiveSidebarPriority', this.mSidebarPriority)
        this.broadcast('VaLayoutManagerConfig', 'Va@configReceiveMinibarPriority', this.mMinibarPriority)
        this.broadcast('VaLayoutManagerConfig', 'Va@configReceiveTopbarPriority', this.mTopbarPriority)
        this.broadcast('VaLayoutManagerConfig', 'Va@configReceiveDesktopMargin', this.mDesktopMargin)
        this.broadcast('VaLayoutManagerConfig', 'Va@configReceiveTopbarPadded', this.mTopbarPadded)
        this.broadcast('VaLayoutManagerConfig', 'Va@configReceiveMinibarTheme', this.mMinibarTheme)
        this.broadcast('VaLayoutManagerConfig', 'Va@configReceiveSidebarTheme', this.mSidebarTheme)
        this.broadcast('VaLayoutManagerConfig', 'Va@configReceiveTopbarTheme', this.mTopbarTheme)
        this.broadcast('VaLayoutManagerConfig', 'Va@configReceivePageBgColor', this.mPageBgColor)
        this.broadcast('VaLayoutManagerConfig', 'Va@configReceiveShowToggle', this.mShowToggle)
        this.broadcast('VaLayoutManagerConfig', 'Va@configReceivePageSize', this.mPageSize)
        this.broadcast('VaLayoutManagerConfig', 'Va@configReceiveCompact', this.mCompact)
        this.broadcast('VaLayoutManagerConfig', 'Va@configReceiveReverse', this.mReverse)
        this.broadcast('VaLayoutManagerConfig', 'Va@configReceiveTextLinks', this.mTextLinks)
        this.broadcast('VaLayoutManagerConfig', 'Va@configReceiveBgColor', this.mBgColor)
        this.broadcast('VaLayoutManagerConfig', 'Va@configReceiveSplit', this.mSplit)
        this.broadcast('VaLayoutManagerConfig', 'Va@configReceiveRtl', this.mRtl)
      }, 100)
    }
  },
  mounted () {
    window.addEventListener('resize', this._handleResize, false)
    /**
     * Call the handler function directly instead of instantiating a resize
     * event like so: window.dispatchEvent(new Event('resize'))
     *
     * Benefit is.. ? Browser compatibility? I dunno.
     */
    this._handleResize()

    /**
     * Mobility check.
     */
    this.setAndBroadcastDimensions()

    /**
     * Presence checks. As of now, this is not really used for
     * anything. Maybe in the future?
     */
    this.checkForPresenceOfSidebar()
    this.checkForPresenceOfTopbar()
    this.checkForPresenceOfPage()

    /**
     * Broadcast props that were passed to the components
     * that need to do stuff based on their values.
     */
    this.broadcastDesktopMinimumWidth(this.mDesktopMinimumWidth)
    this.broadcastMinibarPriority(this.mMinibarPriority)
    this.broadcastSidebarPriority(this.mSidebarPriority)
    this.broadcastTopbarPriority(this.mTopbarPriority)
    this.broadcastDesktopMargin(this.mDesktopMargin)
    this.broadcastTopbarPadded(this.mTopbarPadded)
    this.broadcastContentWidth(this.contentWidth)
    this.broadcastPageBgColor(this.mPageBgColor)
    this.broadcastWindowWidth(this.windowWidth)
    this.broadcastIsReverse(this.mReverse)
    this.broadcastIsSplit(this.mSplit)
    this.broadcastIsRTL(this.mRtl)

    /**
     * --------------------------------------------------
     * Methods related to VaLayoutManagerConfig
     *
     * enableReceivers() exists so that VaLayoutManagerConfig
     * can talk to VaLayoutManager.
     */
    this.enableReceivers()

    /**
     * broadcastDefaultsToConfig() sends default values to
     * VaLayoutManagerConfig, in case some values weren't specified.
     */
    this.broadcastDefaultsToConfig()
  },
  beforeDestroy () {
    window.removeEventListener('resize', this._handleResize, false)
  },
  created () {
    // Presence replies
    this.$on('Va@minibarPresenceReply', (val) => { if (val === true) { this.haveMinibar = true } })
    this.$on('Va@sidebarPresenceReply', (val) => { if (val === true) { this.haveSidebar = true } })
    this.$on('Va@topbarPresenceReply', (val) => { if (val === true) { this.haveTopbar = true } })
    this.$on('Va@pagePresenceReply', (val) => { if (val === true) { this.havePage = true } })

    // Listen for Sidebar width change - implemented when Sidebar's resizer was implemented
    // this.$on('Va@sidebarWidthChange', (val) => {
    //   this.broadcastSidebarWidth(val)
    // })

    // Some components might want to request the isMobile status, for example,
    // if they have been created a while after isMobile was broadcast.
    // See VaRange.
    this.$on('Va@requestIsMobile', (val) => { if (val === true) { this.broadcastIsMobile(this.pastMobileBreakpoint) } })

    // Disconnections
    this.$on('Va@sidebarDisconnect', (val) => { if (val === true) { this.haveSidebar = false } })
    this.$on('Va@minibarDisconnect', (val) => { if (val === true) { this.haveMinibar = false } })
    this.$on('Va@topbarDisconnect', (val) => { if (val === true) { this.haveTopbar = false } })
    this.$on('Va@pageDisconnect', (val) => { if (val === true) { this.havePage = false } })
  },
  computed: {
    styleObj () {
      let bg = this.mBgColor
      let style = {}

      style['background'] = bg
      style['position'] = 'fixed'
      style['top'] = '0px'
      style['right'] = '0px'
      style['bottom'] = '0px'
      style['left'] = '0px'
      style['overflow'] = 'scroll'

      return style
    }
  }
}
</script>
