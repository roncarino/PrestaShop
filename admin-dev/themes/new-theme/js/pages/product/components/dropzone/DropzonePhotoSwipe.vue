<!--**
 * Copyright since 2007 PrestaShop SA and Contributors
 * PrestaShop is an International Registered Trademark & Property of PrestaShop SA
 *
 * NOTICE OF LICENSE
 *
 * This source file is subject to the Open Software License (OSL 3.0)
 * that is bundled with this package in the file LICENSE.md.
 * It is also available through the world-wide-web at this URL:
 * https://opensource.org/licenses/OSL-3.0
 * If you did not receive a copy of the license and are unable to
 * obtain it through the world-wide-web, please send an email
 * to license@prestashop.com so we can send you a copy immediately.
 *
 * DISCLAIMER
 *
 * Do not edit or add to this file if you wish to upgrade PrestaShop to newer
 * versions in the future. If you wish to customize PrestaShop for your
 * needs please refer to https://devdocs.prestashop.com/ for more information.
 *
 * @author    PrestaShop SA and Contributors <contact@prestashop.com>
 * @copyright Since 2007 PrestaShop SA and Contributors
 * @license   https://opensource.org/licenses/OSL-3.0 Open Software License (OSL 3.0)
 *-->
<template>
  <div
    class="pswp"
    tabindex="-1"
    role="dialog"
    aria-hidden="true"
  >
    <div class="pswp__bg" />

    <div class="pswp__scroll-wrap">
      <div class="pswp__container">
        <div class="pswp__item" />
        <div class="pswp__item" />
        <div class="pswp__item" />
      </div>

      <div class="pswp__ui pswp__ui--hidden">
        <div class="pswp__top-bar">
          <div class="pswp__counter" />

          <button
            class="pswp__button pswp__button--close"
            :title="$t('window.closePhotoSwipe')"
          />

          <button
            class="pswp__button pswp__button--share"
            :title="$t('window.download')"
          />

          <button
            class="pswp__button pswp__button--fs"
            :title="$t('window.toggleFullscreen')"
          />

          <button
            class="pswp__button pswp__button--zoom"
            :title="$t('window.zoomPhotoSwipe')"
          />

          <div class="pswp__preloader">
            <div class="pswp__preloader__icn">
              <div class="pswp__preloader__cut">
                <div class="pswp__preloader__donut" />
              </div>
            </div>
          </div>
        </div>

        <div
          class="pswp__share-modal pswp__share-modal--hidden pswp__single-tap"
        >
          <div class="pswp__share-tooltip" />
        </div>

        <button
          class="pswp__button pswp__button--arrow--left"
          :title="$t('window.previousPhotoSwipe')"
        />

        <button
          class="pswp__button pswp__button--arrow--right"
          :title="$t('window.nextPhotoSwipe')"
        />

        <div class="pswp__caption">
          <div class="pswp__caption__center" />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  import PhotoSwipe from 'photoswipe';
  // eslint-disable-next-line
  import PhotoSwipeUI_Default from "photoswipe/dist/photoswipe-ui-default";

  export default {
    name: 'DropzonePhotoSwipe',
    props: {
      files: {
        type: Array,
        default: () => [],
      },
    },
    mounted() {
      const pswpElement = document.querySelectorAll('.pswp')[0];

      const options = {
        index: 0,
        shareButtons: [
          {
            id: 'download',
            label: this.$t('window.downloadImage'),
            url: '{{raw_image_url}}',
            download: true,
          },
        ],
      };

      // This is needed to make our files compatible for photoswipe
      const items = this.files.map((file) => {
        file.src = file.dataURL;
        file.h = file.height;
        file.w = file.width;

        return file;
      });

      const gallery = new PhotoSwipe(
        pswpElement,
        PhotoSwipeUI_Default,
        items,
        options,
      );

      gallery.init();

      const buttons = document.querySelectorAll('.pswp button');

      // We need to disable button default behavior overwise it submit the product form
      buttons.forEach((button) => {
        button.addEventListener('click', (event) => {
          event.preventDefault();
        });
      });

      // We must tell to the rich component that the gallery have been closed
      gallery.listen('destroy', () => {
        this.$emit('closeGallery');
      });
    },
    methods: {},
  };
</script>
