# hjh
web
<!DOCTYPE html>
<html lang="zh-CN">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>实验7：课程管理 </title>
  <style>
    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
      font-family: Arial, sans-serif;
    }
    body {
      background: #f5f7fa;
      padding: 20px 0;
    }
    .app {
      width: 90%;
      max-width: 1000px;
      margin: 0 auto;
      padding: 20px;
      background: white;
      border-radius: 10px;
    }
    .header {
      background: #4096ff;
      color: white;
      padding: 16px;
      text-align: center;
      border-radius: 8px;
      margin-bottom: 20px;
      position: sticky;
      top: 0;
      z-index: 10;
    }
    .search-box, .add-box {
      display: flex;
      gap: 10px;
      margin-bottom: 12px;
    }

    /* 输入框基础样式 */
    input {
      flex: 1;
      padding: 10px 14px;
      border: 1px solid #ddd;
      border-radius: 6px;
      outline: none;
      font-size: 14px;
    }

    input:focus {
      border-color: #4096ff;
      box-shadow: 0 0 0 2px rgba(64, 150, 255, 0.2);
    }

    button {
      padding: 10px 16px;
      background: #4096ff;
      color: white;
      border: none;
      border-radius: 6px;
      cursor: pointer;
    }
    .count {
      color: #666;
      margin-bottom: 16px;
    }
    .course-list {
      display: grid;
      grid-template-columns: repeat(auto-fill, minmax(260px, 1fr));
      gap: 16px;
      margin-bottom: 30px;
    }
    .card {
      border: 1px solid #eee;
      padding: 16px;
      border-radius: 8px;
    }
    .btns {
      display: flex;
      gap: 8px;
      margin-top: 10px;
    }
    .btns button:last-child {
      background: #ff4d4f;
    }
    .footer {
      text-align: center;
      color: #999;
      padding-top: 20px;
      border-top: 1px solid #eee;
    }
  </style>
</head>
<body>
  <div class="app">
    <div class="header"><h1>React Hooks 课程管理</h1></div>
    <div class="search-box">
      <input type="text" id="searchInput" placeholder="搜索课程" />
    </div>
    <div class="add-box">
      <input type="text" id="courseInput" placeholder="输入课程名称" />
      <button onclick="addCourse()">添加课程</button>
    </div>
    <div class="count" id="courseCount">总课程数：0 门</div>
    <div class="course-list" id="courseList"></div>
    <div class="footer">实验7 © 2026</div>
  </div>

  <script>
    let courses = JSON.parse(localStorage.getItem('courses')) || [
      { id: 1, title: 'React 基础', desc: 'JSX、组件、状态' },
      { id: 2, title: 'React Hooks', desc: 'useState、useEffect' },
      { id: 3, title: '组件通信', desc: 'props 传值' },
    ];
    let searchText = '';

    function saveAndRender() {
      localStorage.setItem('courses', JSON.stringify(courses));
      render();
    }

    function render() {
      const list = document.getElementById('courseList');
      const count = document.getElementById('courseCount');
      const filtered = courses.filter(item =>
        item.title.includes(searchText)
      );

      list.innerHTML = '';
      filtered.forEach(item => {
        const card = document.createElement('div');
        card.className = 'card';
        card.innerHTML = `
          <h3>${item.title}</h3>
          <p>${item.desc}</p>
          <div class="btns">
            <button onclick="learn(${item.id})">学习</button>
            <button onclick="del(${item.id})">删除</button>
          </div>
        `;
        list.appendChild(card);
      });
      count.innerText = `总课程数：${filtered.length} 门`;
    }

    // 添加 + 自动聚焦
    function addCourse() {
      const input = document.getElementById('courseInput');
      const title = input.value.trim();

      if (!title) {
        alert('课程名不能为空');
        input.focus();
        return;
      }

      courses.push({ id: Date.now(), title, desc: '新增课程' });
      input.value = '';
      saveAndRender();
      input.focus(); // 自动聚焦
    }

    function learn(id) {
      const c = courses.find(x => x.id === id);
      alert('开始学习：' + c.title);
    }

    function del(id) {
      courses = courses.filter(x => x.id !== id);
      saveAndRender();
    }

    document.getElementById('searchInput').addEventListener('input', e => {
      searchText = e.target.value;
      render();
    });

    document.getElementById('courseInput').addEventListener('keydown', e => {
      if (e.key === 'Enter') addCourse();
    });

    render();
  </script>
</body>
</html>
