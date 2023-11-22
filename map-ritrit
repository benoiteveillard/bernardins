if (typeof epic !== "object") {
  var epic = {};
}
epic.map = {
  x: {
    x: "epic-map-",
    y: {
      map: {
        x: "map",
        y: {
          ops: "options",
          bnd: "boundsoptions",
          sty: "style",
          lng: "lng",
          lat: "lat",
          zom: "zoom",
          nav: "nav",
          ugo: "usergeo",
        },
      },
      lst: {
        x: "list",
        y: {
          adr: "address",
          lng: "lng",
          lat: "lat",
          mrk: "marker",
          pop: "popup",
          onh: "onhover",
          onc: "onclick",
        },
      },
      mrk: {
        x: "marker",
        y: {
          ops: "options",
          onh: "onhover",
          onc: "onclick",
        },
      },
      pop: {
        x: "popup",
        y: {
          ops: "options",
          cls: "closebutton",
        },
      },
    },
  },
  instances: {},
  options: {
    map: {
      style: "mapbox://styles/mapbox/streets-v11",
      center: { lng: -2.36, lat: 51.38 },
      zoom: 4,
    },
    bounds: {
      desktop: { padding: 64, maxZoom: 16 },
      mobile: { padding: 32, maxZoom: 16 },
    },
    marker: { color: "#ee3c49" },
    popup: { focusAfterOpen: false, closeButton: true },
  },
  getMapboxOptions: (x, y) => {
    if (typeof x === "number") {
      x = x.toString();
    }
    if (typeof y === "number") {
      y = y.toString();
    }
    if (typeof x !== "string") {
      if (typeof y !== "string") {
        return {};
      } else {
        x = y;
      }
    }
    let options = epic.map.options,
      ops = {};
    if (
      typeof epicmapoptions === "object" &&
      epicmapoptions.hasOwnProperty(x)
    ) {
      options = epicmapoptions;
    } else if (!epic.map.options.hasOwnProperty(x)) {
      if (typeof y === "string" && epic.map.options.hasOwnProperty(y)) {
        x = y;
      } else {
        return {};
      }
    }
    for (z in options[x]) {
      ops[z] = options[x][z];
    }
    return ops;
  },
  getOptions: (el, x) => {
    if (typeof el !== "object") {
      return {};
    }
    if (typeof x !== "string") {
      x = $(el).attr(epic.map.x.el.x);
    }
    if (typeof x !== "string" || x === "") {
      return ops;
    }
    for (y in epic.map.x.y) {
      if (x === epic.map.x.y[y].x) {
        x = y;
      }
    }
    let a = {};
    for (z in epic.map.x.el[x].y) {
      a[z] = epic.map.x.el[x].y[z];
    }
    if (x === "lst") {
      for (xy in epic.map.x.y) {
        let y = epic.map.x.y[xy];
        if (y.x !== epic.map.x.y.mrk.x && y.x !== epic.map.x.y.pop.x) {
          continue;
        }
        for (z in y.y) {
          a[y.x.charAt(0) + z] = epic.map.x.x + y.x + y.y[z];
        }
      }
    }
    let ops = {};
    for (y in a) {
      let z = a[y].replace(epic.map.x.x, "");
      ops[z] = false;
      if (el.hasAttribute(a[y])) {
        ops[z] = el.getAttribute(a[y]);
      }
      if (typeof ops[z] === "boolean") {
      } else if (ops[z] === undefined || ops[z] === null) {
        ops[z] = false;
      } else if (!isNaN(ops[z]) && ops[z] !== "") {
        ops[z] = Number(ops[z]);
      }
      if (z === epic.map.x.y.lst.y.adr) {
        if (typeof ops[z] !== "string" || ops[z] === "") {
          ops[z] = false;
        }
      }
      if (z === epic.map.x.y.lst.y.lng || z === epic.map.x.y.lst.y.lat) {
        if (typeof ops[z] !== "number") {
          ops[z] = false;
        }
      }
      if (z.slice(-7) === "options") {
        let og = z.slice(0, -7);
        if (z === "options") {
          og = x;
        }
        ops[z] = epic.map.getMapboxOptions(ops[z], og);
      }
    }
    return ops;
  },
  getInstance: (id) => {
    if (typeof id === "number") {
      id = id.toString();
    } else if (typeof id !== "string") {
      id = "0";
    }
    let inst = false;
    if (epic.map.instances.hasOwnProperty(id)) {
      inst = epic.map.instances[id];
    }
    return inst;
  },
  getItem: (id, itid) => {
    let inst = epic.map.getInstance(id);
    if (inst === false) {
      return;
    }
    if (typeof itid !== "string") {
      return;
    }
    if (!inst.items.hasOwnProperty(itid)) {
      return;
    }
    return inst.items[itid];
  },
  fitBounds: (id) => {
    let inst = epic.map.getInstance(id);
    if (inst === false) {
      return;
    }
    $(inst.map).each(function (i) {
      let x = epic.map.x.y.map.y.bnd;
      if (!inst.map[i].options.hasOwnProperty(x)) {
        return;
      }
      x = inst.map[i].options[x];
      let ops = x;
      if (x.hasOwnProperty("desktop")) {
        ops = x.desktop;
      }
      if (window.innerWidth <= 991 && x.hasOwnProperty("tablet")) {
        ops = x.tablet;
      }
      if (window.innerWidth <= 767 && x.hasOwnProperty("mobile")) {
        ops = x.mobile;
      }
      inst.map[i].x.fitBounds(inst.bounds, ops);
    });
  },
  updateMarkers: (id) => {
    let inst = epic.map.getInstance(id);
    if (inst === false) {
      return;
    }
    let bounds = false;
    inst.bounds = new mapboxgl.LngLatBounds();
    for (itid in inst.items) {
      let it = inst.items[itid];
      if (it.markers === false || it.markers.length === 0) {
        continue;
      }
      it.markers.forEach((mrk) => {
        if (mrk.popup !== false && mrk.popup.hasOwnProperty("remove")) {
          mrk.popup.remove();
        }
        if (inst.activeItems.includes(itid)) {
          mrk.marker._element.style.removeProperty("display");
        } else {
          mrk.marker._element.style.display = "none";
        }
      });
      if (inst.activeItems.includes(itid)) {
        inst.bounds.extend([it.options.lng, it.options.lat]);
        bounds = true;
      }
    }
    if (bounds) {
      epic.map.fitBounds(id);
    }
  },
  initActions: (itid, x, id) => {
    let inst = epic.map.getInstance(id);
    let it = epic.map.getItem(id, itid);
    if (it === false) {
      return;
    }
    if (!inst.hasOwnProperty("map")) {
      return;
    }
    if (typeof x !== "boolean") {
      x = true;
    }
    function popup(y, z) {
      for (a in inst.items) {
        if (!inst.items[a].hasOwnProperty("markers")) {
          continue;
        }
        if (z && a === y) {
          inst.items[a].markers.forEach((b) => {
            inst.map.forEach((c) => {
              b.popup.addTo(c.x);
            });
          });
        } else {
          inst.items[a].markers.forEach((b) => {
            b.popup.remove();
          });
        }
      }
    }
    function goto(y) {
      for (a in inst.items) {
        if (!inst.items[a].hasOwnProperty("markers")) {
          continue;
        }
        if (a === y) {
          inst.items[a].markers.forEach((b) => {
            inst.map.forEach((c) => {
              c.x.panTo(b.marker._lngLat);
            });
          });
        }
      }
    }
    let itops = it.options;
    if (x && itops.onhover === "popup") {
      it.el.addEventListener("mouseover", () => {
        popup(itid, true);
      });
      it.el.addEventListener("mouseout", () => {
        popup(itid, false);
      });
    }
    if (x && itops.onclick === "goto") {
      it.el.addEventListener("click", () => {
        goto(itid);
      });
    }
    if (
      it.hasOwnProperty("markers") &&
      itops.hasOwnProperty(epic.map.x.y.mrk.x) &&
      itops.marker.hasOwnProperty("options")
    ) {
      let mrkops = itops.marker.options;
      if (mrkops.hasOwnProperty("onhover")) {
        if (mrkops.onhover === "popup") {
          it.markers.forEach((y) => {
            if (!y.hasOwnProperty("marker")) {
              return;
            }
            if (!y.marker.hasOwnProperty("_element")) {
              return;
            }
            y.marker._element.addEventListener("mouseover", () => {
              popup(itid, true);
            });
            y.marker._element.addEventListener("mouseout", () => {
              popup(itid, false);
            });
          });
        }
      }
    }
  },
  initFilters: (id) => {
    let inst = epic.map.getInstance(id);
    if (inst === false) {
      return;
    }
    window.fsAttributes = window.fsAttributes || [];
    window.fsAttributes.push([
      "cmsfilter",
      (filterInstances) => {
        filterInstances.forEach((filterInstance) => {
          let ls = filterInstance.listInstance.list;
          filterInstance.listInstance.on("renderitems", (renderedItems) => {
            let lsid = $(ls).attr(epic.map.x.id);
            if (lsid === id) {
              inst.activeItems = [];
              renderedItems.forEach((it) => {
                inst.activeItems.push($(it.element).attr(epic.map.x.id));
              });
              epic.map.initList(id);
              epic.map.updateMarkers(id);
            }
          });
        });
      },
    ]);
    window.fsAttributes.push([
      "cmsload",
      (listInstances) => {
        let [listInstance] = listInstances;
        let ls = listInstance.list;
        listInstance.on("renderitems", (renderedItems) => {
          let lsid = $(ls).attr(epic.map.x.id);
          if (lsid === id) {
            inst.activeItems = [];
            renderedItems.forEach((it) => {
              inst.activeItems.push($(it.element).attr(epic.map.x.id));
            });
            epic.map.initList(id);
            epic.map.updateMarkers(id);
          }
        });
      },
    ]);
  },
  initMarker: (id, itid) => {
    let inst = epic.map.getInstance(id);
    if (inst === false) {
      return;
    }
    if (!inst.hasOwnProperty(epic.map.x.y.map.x)) {
      return;
    }
    let it = epic.map.getItem(id, itid);
    let itops = it.options;
    if (it === false || itops.lng === false || itops.lat === false) {
      return;
    }
    let mrkops = itops.marker.options;
    let popops = itops.popup.options;
    if (popops[epic.map.x.y.pop.y.cls] === "true") {
      popops.options.closeButton = true;
    } else if (popops[epic.map.x.y.pop.y.cls] === "false") {
      popops.options.closeButton = false;
    }
    $(inst.map).each(function (i) {
      let x = {
        marker: new mapboxgl.Marker(mrkops.options)
          .setLngLat([itops.lng, itops.lat])
          .addTo(inst.map[i].x),
        popup: false,
      };
      if (
        itops.hasOwnProperty(epic.map.x.y.pop.x) &&
        typeof itops[epic.map.x.y.pop.x].el === "object"
      ) {
        x.popup = new mapboxgl.Popup(popops.options).setDOMContent(
          itops.popup.el.cloneNode(true)
        );
        x.marker.setPopup(x.popup);
      }
      it.markers.push(x);
    });
    inst.bounds.extend([itops.lng, itops.lat]);
    epic.map.fitBounds(id);
    epic.map.initActions(itid, false, id);
  },
  initList: (id) => {
    let inst = epic.map.getInstance(id);
    if (inst === false) {
      return;
    }
    if (!inst.hasOwnProperty(epic.map.x.y.lst.x)) {
      inst[epic.map.x.y.lst.x] = [];
    }
    $(inst.list).each(function (i) {
      i = i.toString();
      $(inst.list[i].el)
        .children()
        .each(function (j) {
          let itid = i + "-" + j.toString();
          if (this.hasAttribute(epic.map.x.id)) {
            itid = this.getAttribute(epic.map.x.id);
          } else {
            $(this).attr(epic.map.x.id, itid);
          }
          if (inst.items.hasOwnProperty(itid)) {
            return;
          }
          let it = {
            el: this,
            markers: [],
            options: epic.map.getOptions(this, epic.map.x.y.lst.x),
          };
          let itops = it.options;
          for (x in inst.list[i].options) {
            if (itops[x] === false) {
              itops[x] = inst.list[i].options[x];
            }
          }
          let els = [epic.map.x.el.mrk.x, epic.map.x.el.pop.x];
          els.forEach((elx) => {
            let x = elx
              .slice(1, -1)
              .replace(epic.map.x.el.x + "=", "")
              .slice(1, -1);
            let xops = x + "options",
              ops = {};
            if (itops[x] === "") {
              itops[x] = false;
            }
            if (x === epic.map.x.y.mrk.x && typeof itops[x] === "string") {
              if (itops[x].charAt(0) === "#") {
                ops.color = itops[x];
              } else {
                ops.element = document.createElement("div");
                $(ops.element).addClass(itops[x]);
              }
            }
            for (y in ops) {
              itops[xops][y] = ops[y];
            }
            let el = this.querySelector(elx);
            if (el === undefined || el === null) {
              el = false;
            } else if (x === epic.map.x.y.mrk.x) {
              itops[xops].element = el.cloneNode(true);
            }
            itops[x] = {
              el: el,
              options: epic.map.getOptions(el),
            };
            if (!itops[x].options.hasOwnProperty(epic.map.x.ops)) {
              itops[x].options.options = itops[xops];
            } else {
              for (y in itops[xops]) {
                let z = itops[x].options.options;
                if (!z.hasOwnProperty(y) || z[y] === false) {
                  z[y] = itops[xops][y];
                }
              }
            }
            for (y in itops) {
              if (y.slice(0, x.length) !== x) {
                continue;
              }
              let z = y.slice(x.length);
              if (z === "" || z === "options") {
                continue;
              }
              itops[x].options[z] = itops[y];
            }
          });
          inst.items[itid] = it;
          if (itops.lng !== false && itops.lat !== false) {
            epic.map.initMarker(id, itid);
          } else if (itops.address !== false) {
            let url =
              "https://api.mapbox.com/geocoding/v5/mapbox.places/" +
              itops.address +
              ".json?limit=1&access_token=" +
              mapboxgl.accessToken;
            let xhr = new XMLHttpRequest();
            xhr.addEventListener("load", () => {
              let res = JSON.parse(xhr.response);
              if (
                res.hasOwnProperty("features") &&
                res.features[0].hasOwnProperty("geometry") &&
                res.features[0].geometry.hasOwnProperty("coordinates")
              ) {
                let lnglat = res.features[0].geometry.coordinates;
                epic.map.instances[id].items[itid].options.lng = lnglat[0];
                epic.map.instances[id].items[itid].options.lat = lnglat[1];
                epic.map.initMarker(id, itid);
                epic.map.initActions(itid, false, id);
              }
            });
            xhr.open("GET", url, true);
            xhr.send();
          }
        });
    });
    for (itid in inst.items) {
      epic.map.initActions(itid, true, id);
    }
  },
  initMap: (id) => {
    let inst = epic.map.getInstance(id);
    if (inst === false) {
      return;
    }
    if (!inst.hasOwnProperty(epic.map.x.y.map.x)) {
      return;
    }
    $(inst.map).each(function (i) {
      let ops = inst.map[i].options;
      ops.options.container = inst.map[i].el;
      if (
        typeof ops[epic.map.x.y.map.y.sty] === "string" &&
        ops[epic.map.x.y.map.y.sty].slice(0, 16) === "mapbox://styles/"
      ) {
        ops.options.style = ops[epic.map.x.y.map.y.sty];
      }
      if (
        typeof ops[epic.map.x.y.map.y.lng] === "number" &&
        typeof ops[epic.map.x.y.map.y.lat] === "number"
      ) {
        ops.options.center = [
          ops[epic.map.x.y.map.y.lng],
          ops[epic.map.x.y.map.y.lat],
        ];
      }
      if (typeof ops[epic.map.x.y.map.y.zom] === "number") {
        ops.options.zoom = ops[epic.map.x.y.map.y.zom];
      }
      inst.map[i].x = new mapboxgl.Map(ops.options);
      if (ops[epic.map.x.y.map.y.nav] !== "false") {
        inst.map[i].x.addControl(new mapboxgl.NavigationControl());
      }
      if (ops[epic.map.x.y.map.y.ugo] === "true") {
        inst.map[i].x.addControl(
          new mapboxgl.GeolocateControl({
            positionOptions: { enableHighAccuracy: true },
          })
        );
      }
    });
  },
  initAttrs: () => {
    epic.map.x.el = { x: epic.map.x.x + "element" };
    epic.map.x.id = epic.map.x.x + "id";
    for (x in epic.map.x.y) {
      epic.map.x.el[x] = {
        x: "[" + epic.map.x.el.x + "='" + epic.map.x.y[x].x + "']",
        y: {},
      };
      for (y in epic.map.x.y[x].y) {
        epic.map.x.el[x].y[y] = epic.map.x.x + epic.map.x.y[x].y[y];
      }
    }
  },
  init: () => {
    epic.map.initAttrs();
    $(
      epic.map.x.el.map.x.replace("=", "|=") +
        ", " +
        epic.map.x.el.lst.x.replace("=", "|=")
    ).each(function () {
      let x = this.getAttribute(epic.map.x.el.x),
        id = "_main";
      let mapl = epic.map.x.y.map.x.length,
        lstl = epic.map.x.y.lst.x.length;
      if (x.slice(0, mapl) === epic.map.x.y.map.x) {
        if (x.slice(mapl).length >= 2) {
          id = x.slice(mapl + 1);
        }
        x = x.slice(0, mapl);
        $(this).attr(epic.map.x.el.x, x.slice(0, mapl));
      } else if (x.slice(lstl).length >= 2) {
        id = x.slice(lstl + 1);
        x = x.slice(0, lstl);
        $(this).attr(epic.map.x.el.x, x.slice(0, lstl));
      }
      if (!epic.map.instances.hasOwnProperty(id)) {
        epic.map.instances[id] = {};
      }
      if (!epic.map.instances[id].hasOwnProperty(x)) {
        epic.map.instances[id][x] = [];
      }
      epic.map.instances[id][x].push({
        el: this,
        options: epic.map.getOptions(this),
      });
      $(this).attr(epic.map.x.id, id);
    });
    for (id in epic.map.instances) {
      epic.map.instances[id].items = {};
      epic.map.instances[id].activeItems = [];
      epic.map.instances[id].bounds = new mapboxgl.LngLatBounds();
      epic.map.initMap(id);
      epic.map.initList(id);
      epic.map.initFilters(id);
    }
  },
  preflight: () => {
    var x = [false, false];
    document.addEventListener("DOMContentLoaded", () => {
      x[0] = true;
      if (x[1] === true) {
        epic.map.init();
      }
    });
    var y =
      "pk.eyJ1IjoiZWRvdWFyZGZheSIsImEiOiJjbG4wZzVtbmIwbzI3Mm5sMTY1ZTVrZnUwIn0.rywvMUh2iFjeghDQb_fZPg";

    const a = document.createElement("script");
    a.setAttribute("async", "false");
    a.src = "https://api.mapbox.com/mapbox-gl-js/v2.15.0/mapbox-gl.js";
    const b = document.createElement("link");
    b.rel = "stylesheet";
    b.href = "https://api.mapbox.com/mapbox-gl-js/v2.15.0/mapbox-gl.css";
    const c = document.createElement("style");
    c.type = "text/css";
    const d =
      ".mapboxgl-popup-content {background:none;border-radius:0px;box-shadow:none;padding:0px}";
    if (c.styleSheet) {
      c.styleSheet.cssText = d;
    } else {
      c.appendChild(document.createTextNode(d));
    }
    document.head.append(a);
    document.head.append(b);
    document.head.append(c);
    a.addEventListener("load", () => {
      mapboxgl.accessToken = y;
      x[1] = true;
      if (x[0] === true) {
        epic.map.init();
      }
    });
  },
};
epic.map.preflight();
