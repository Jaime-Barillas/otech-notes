# Ontario Tech University - Notes

See the side bar for the list of available pages.

## Canvas Bookmarklet

Makes the document previews usable.
~~~javascript
javascript:(() => {
  let d = document.createElement('div');
  d.id = 'bookmarklet-spacing';
  d.style.height = window.screen.height.toString() + 'px';
  d.style.padding = '2rem';
  d.style.display = 'flex';
  d.style.flexFlow = 'column nowrap';
  d.style.alignItems = 'center';
  d.style.justifyContent = 'space-between';

  let s1 = document.createElement('span');
  s1.textContent = 'Space left intentionally blank';
  let s2 = s1.cloneNode(true);

  d.appendChild(s1);
  d.appendChild(s2);
  document.body.appendChild(d);

  document.body.classList.remove('course-menu-expanded');
  let nav = document.querySelector('#left-side');
  nav.style.display = 'none';

  let footer = document.querySelector('#sequence_footer');
  footer.remove();

  let viewport = document.querySelector('#doc_preview > div');
  viewport.style.height = (viewport.clientHeight * 2).toString() + 'px';
})();
~~~
