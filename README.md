<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>UXUI 디자이너 프로젝트 섹션</title>
    <style>
        @import url('https://cdn.jsdelivr.net/gh/orioncactus/pretendard/dist/web/static/pretendard.css');
        
        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: 'Pretendard', -apple-system, sans-serif;
        }
        
        body {
            background-color: #f8f9fa;
            color: #343a40;
            line-height: 1.6;
            padding: 20px;
        }
        
        .planner-container {
            max-width: 800px;
            margin: 20px auto;
            background: white;
            box-shadow: 0 10px 30px rgba(0,0,0,0.05);
            border-radius: 20px;
            overflow: hidden;
        }
        
        .planner-section {
            padding: 25px 35px;
            border-bottom: 1px solid #e9ecef;
        }
        
        .section-title {
            display: flex;
            align-items: center;
            margin-bottom: 20px;
            font-size: 18px;
            font-weight: 600;
            color: #495057;
        }
        
        .section-title:before {
            content: '';
            display: inline-block;
            width: 12px;
            height: 12px;
            background: linear-gradient(135deg, #4158D0, #C850C0);
            margin-right: 10px;
            border-radius: 50%;
        }
        
        /* 프로젝트 진행도 표시 스타일 */
        .project-progress-container {
            position: relative;
            margin: 15px 0;
        }
        
        .project-progress-bar {
            height: 8px;
            background: #e9ecef;
            border-radius: 10px;
            overflow: hidden;
            position: relative;
        }

        .project-progress-bar-inner {
            height: 100%;
            background: linear-gradient(135deg, #4158D0, #C850C0);
            width: 45%;
            border-radius: 10px;
        }
        
        .progress-handle {
            position: absolute;
            width: 20px;
            height: 20px;
            background: white;
            border: 3px solid #4158D0;
            border-radius: 50%;
            top: 50%;
            transform: translate(-50%, -50%);
            left: 45%; /* 초기 위치, JS로 업데이트됨 */
            cursor: pointer;
            z-index: 10;
            box-shadow: 0 2px 5px rgba(0,0,0,0.2);
            transition: box-shadow 0.2s;
        }
        
        .progress-handle:hover {
            box-shadow: 0 3px 8px rgba(0,0,0,0.3);
        }

        .enhanced-project-section {
            background: #f8f9fa;
            border-radius: 15px;
            padding: 20px;
            margin-bottom: 20px;
            position: relative;
        }

        .project-task-container {
            margin-top: 15px;
        }

        .project-task {
            background: white;
            border-radius: 10px;
            padding: 15px;
            margin-bottom: 10px;
            box-shadow: 0 2px 5px rgba(0,0,0,0.05);
        }

        .project-task-header {
            display: flex;
            justify-content: space-between;
            margin-bottom: 10px;
        }

        /* 작업 상태 스타일 */
        .task-status-select {
            padding: 3px 8px;
            border-radius: 15px;
            font-size: 12px;
            font-weight: 500;
            border: none;
            appearance: none;
            -webkit-appearance: none;
            -moz-appearance: none;
            background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='8' height='8' viewBox='0 0 8 8'%3E%3Cpath fill='%23343a40' d='M.5 1.5l3 3 3-3'/%3E%3C/svg%3E");
            background-repeat: no-repeat;
            background-position: right 8px center;
            background-size: 8px;
            padding-right: 24px;
            cursor: pointer;
            outline: none;
        }
        
        .status-planned {
            background-color: #d1ecf1;
            color: #0c5460;
        }
        
        .status-in-progress {
            background-color: #fff3cd;
            color: #856404;
        }
        
        .status-completed {
            background-color: #d4edda;
            color: #155724;
        }
        
        /* 모달 스타일 */
        .modal-overlay {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0, 0, 0, 0.5);
            display: flex;
            justify-content: center;
            align-items: center;
            z-index: 1000;
            visibility: hidden;
            opacity: 0;
            transition: all 0.3s;
        }
        
        .modal-overlay.active {
            visibility: visible;
            opacity: 1;
        }
        
        .modal-container {
            background: white;
            width: 90%;
            max-width: 600px;
            border-radius: 20px;
            overflow: hidden;
            box-shadow: 0 15px 30px rgba(0, 0, 0, 0.2);
            transform: scale(0.9);
            transition: all 0.3s;
        }
        
        .modal-overlay.active .modal-container {
            transform: scale(1);
        }
        
        .modal-header {
            background: linear-gradient(135deg, #4158D0, #C850C0);
            color: white;
            padding: 20px;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
        
        .modal-title {
            font-weight: 600;
            font-size: 20px;
        }
        
        .modal-close {
            background: none;
            border: none;
            color: white;
            font-size: 24px;
            cursor: pointer;
            outline: none;
        }
        
        .modal-body {
            padding: 20px;
            max-height: 70vh;
            overflow-y: auto;
        }
        
        .modal-footer {
            padding: 15px 20px;
            display: flex;
            justify-content: flex-end;
            border-top: 1px solid #dee2e6;
            gap: 10px;
        }
        
        .modal-button {
            background: linear-gradient(135deg, #4158D0, #C850C0);
            color: white;
            border: none;
            padding: 10px 20px;
            border-radius: 10px;
            font-weight: 500;
            cursor: pointer;
            transition: all 0.2s;
        }
        
        .modal-button:hover {
            opacity: 0.9;
            transform: translateY(-2px);
        }
        
        .cancel-button {
            background: #e9ecef;
            color: #495057;
        }
        
        .form-group {
            margin-bottom: 15px;
        }
        
        .form-label {
            display: block;
            margin-bottom: 5px;
            font-weight: 500;
            color: #495057;
        }
        
        .form-input {
            width: 100%;
            padding: 10px;
            border: 1px solid #dee2e6;
            border-radius: 8px;
            font-size: 16px;
            outline: none;
            transition: border-color 0.2s;
        }
        
        .form-input:focus {
            border-color: #4158D0;
            box-shadow: 0 0 0 3px rgba(65, 88, 208, 0.1);
        }
        
        .form-textarea {
            min-height: 100px;
            resize: vertical;
        }
        
        .date-inputs {
            display: flex;
            gap: 10px;
        }
        
        .date-inputs .form-group {
            flex: 1;
        }
        
        .add-project-btn {
            background: linear-gradient(135deg, #4158D0, #C850C0);
            color: white;
            border: none;
            padding: 10px 15px;
            border-radius: 10px;
            font-weight: 500;
            font-size: 14px;
            cursor: pointer;
            display: flex;
            align-items: center;
            margin-bottom: 20px;
            transition: all 0.2s;
        }
        
        .add-project-btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 5px 15px rgba(65, 88, 208, 0.2);
        }
        
        .add-project-btn:before {
            content: '+';
            font-size: 18px;
            margin-right: 8px;
        }
        
        .projects-container {
            display: flex;
            flex-direction: column;
            gap: 15px;
        }
        
        /* 삭제 버튼 스타일 */
        .project-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 15px;
        }
        
        .project-title {
            font-weight: 600;
            font-size: 18px;
        }
        
        .project-actions {
            display: flex;
            gap: 8px;
        }
        
        .delete-project-btn {
            background: white;
            color: #6c757d;
            border: 1px solid #adb5bd;
            border-radius: 6px;
            padding: 3px 10px;
            font-size: 12px;
            font-weight: 500;
            cursor: pointer;
            transition: all 0.2s;
        }
        
        .delete-project-btn:hover {
            background: #f8f9fa;
        }
        
        /* 확인 모달 스타일 */
        .confirm-modal .modal-header {
            background: #dc3545;
        }
        
        .confirm-modal .modal-button.danger {
            background: #dc3545;
        }
    </style>
</head>
<body>
    <div class="planner-container">
        <div class="planner-section">
            <h2 class="section-title">UXUI 디자이너 성장 활동</h2>
            
            <button class="add-project-btn" id="openNewProjectModal">새 프로젝트 추가</button>
            
            <div class="projects-container" id="projectsContainer">
                <!-- 기존 프로젝트 컴포넌트 -->
                <div class="enhanced-project-section" data-project-id="project-1">
                    <div class="project-header">
                        <h3 class="project-title">모바일 뱅킹 앱 리디자인</h3>
                        <div class="project-actions">
                            <button class="delete-project-btn" onclick="confirmDeleteProject('project-1')">삭제</button>
                        </div>
                    </div>
                    
                    <div style="display: flex; justify-content: space-between; font-size: 12px; margin-bottom: 5px;">
                        <span>프로젝트 진행도</span>
                        <span class="progress-percent">45%</span>
                    </div>
                    
                    <div class="project-progress-container">
                        <div class="project-progress-bar">
                            <div class="project-progress-bar-inner"></div>
                        </div>
                        <div class="progress-handle"></div>
                    </div>
                    
                    <h4 style="font-weight: 500; margin: 15px 0 10px 0;">오늘의 작업</h4>
                    <div class="project-task-container">
                        <div class="project-task">
                            <div class="project-task-header">
                                <input type="text" value="사용자 조사 결과 분석" style="border: none; background: transparent; outline: none; font-weight: 500; width: 70%;">
                                <select class="task-status-select status-completed" onchange="changeTaskStatus(this)">
                                    <option value="planned">계획됨</option>
                                    <option value="in-progress">진행중</option>
                                    <option value="completed" selected>완료</option>
                                </select>
                            </div>
                            <textarea placeholder="상세 내용 및 얻은 인사이트" style="width: 100%; border: none; resize: none; outline: none; font-size: 14px; min-height: 60px;">15명의 사용자 인터뷰 결과를 분석하여 주요 페인 포인트 3가지를 파악. 사용성 테스트 결과 대시보드 화면의 개선이 필요함.</textarea>
                        </div>
                        
                        <div class="project-task">
                            <div class="project-task-header">
                                <input type="text" value="와이어프레임 스케치" style="border: none; background: transparent; outline: none; font-weight: 500; width: 70%;">
                                <select class="task-status-select status-in-progress" onchange="changeTaskStatus(this)">
                                    <option value="planned">계획됨</option>
                                    <option value="in-progress" selected>진행중</option>
                                    <option value="completed">완료</option>
                                </select>
                            </div>
                            <textarea placeholder="상세 내용 및 얻은 인사이트" style="width: 100%; border: none; resize: none; outline: none; font-size: 14px; min-height: 60px;">메인 화면과 거래 내역 페이지의 와이어프레임 작업 중. 내일까지 완성 목표.</textarea>
                        </div>
                    </div>
                    
                    <h4 style="font-weight: 500; margin: 20px 0 10px 0;">프로젝트 회고</h4>
                    <textarea placeholder="오늘의 프로젝트에서 배운 점, 어려웠던 점, 내일 개선할 점 등을 기록하세요." style="width: 100%; border: 1px solid #dee2e6; border-radius: 10px; padding: 12px; min-height: 100px; outline: none; resize: none;">사용자 인터뷰에서 예상치 못한 피드백을 받아 초기 계획을 수정해야 했다. 
특히 노년층 사용자들의 접근성 요구사항이 중요하게 부각되었고, 이를 와이어프레임에 반영하는 과정이 도전적이었다.

내일은 포커스 그룹을 진행하여 와이어프레임에 대한 피드백을 받을 예정.
배운 점: 사용자 조사의 중요성과 데이터 기반 의사결정의 가치.</textarea>
                </div>
            </div>
        </div>
    </div>
    
    <!-- 새 프로젝트 추가 모달 -->
    <div class="modal-overlay" id="newProjectModal">
        <div class="modal-container">
            <div class="modal-header">
                <h3 class="modal-title">새 프로젝트 추가</h3>
                <button class="modal-close" id="closeNewProjectModal">&times;</button>
            </div>
            <div class="modal-body">
                <form id="newProjectForm">
                    <div class="form-group">
                        <label class="form-label" for="projectName">프로젝트명</label>
                        <input type="text" id="projectName" class="form-input" placeholder="예: 웹사이트 리디자인" required>
                    </div>
                    
                    <div class="date-inputs">
                        <div class="form-group">
                            <label class="form-label" for="startDate">시작일</label>
                            <input type="date" id="startDate" class="form-input" required>
                        </div>
                        <div class="form-group">
                            <label class="form-label" for="endDate">종료 예정일</label>
                            <input type="date" id="endDate" class="form-input" required>
                        </div>
                    </div>
                    
                    <div class="form-group">
                        <label class="form-label" for="projectDescription">프로젝트 설명</label>
                        <textarea id="projectDescription" class="form-input form-textarea" placeholder="프로젝트의 목적, 범위, 주요 목표 등을 기록하세요"></textarea>
                    </div>
                    
                    <div class="form-group">
                        <label class="form-label" for="projectGoals">프로젝트 주요 목표 (최대 3개)</label>
                        <div style="display: flex; flex-direction: column; gap: 8px;">
                            <input type="text" class="form-input" placeholder="목표 1">
                            <input type="text" class="form-input" placeholder="목표 2">
                            <input type="text" class="form-input" placeholder="목표 3">
                        </div>
                    </div>
                </form>
            </div>
            <div class="modal-footer">
                <button class="modal-button cancel-button" id="cancelNewProject">취소</button>
                <button class="modal-button" id="saveNewProject">저장</button>
            </div>
        </div>
    </div>
    
    <!-- 프로젝트 삭제 확인 모달 -->
    <div class="modal-overlay confirm-modal" id="deleteConfirmModal">
        <div class="modal-container">
            <div class="modal-header">
                <h3 class="modal-title">프로젝트 삭제</h3>
                <button class="modal-close" id="closeDeleteConfirmModal">&times;</button>
            </div>
            <div class="modal-body">
                <p>정말로 이 프로젝트를 삭제하시겠습니까?</p>
                <p style="font-size: 14px; color: #dc3545; margin-top: 10px;">이 작업은 되돌릴 수 없습니다.</p>
            </div>
            <div class="modal-footer">
                <button class="modal-button cancel-button" id="cancelDeleteProject">취소</button>
                <button class="modal-button danger" id="confirmDeleteProject">삭제</button>
            </div>
        </div>
    </div>

    <script>
        document.addEventListener('DOMContentLoaded', function() {
            setupProgressBars();
            setupModals();
            setupProjectActions();
            
            // 프로젝트 추가 버튼 이벤트
            document.getElementById('saveNewProject').addEventListener('click', addNewProject);
        });
        
        // 전역 변수로 현재 삭제할 프로젝트 ID 저장
        let currentProjectToDelete = null;
        
        // 모든 진행도 바 설정
        function setupProgressBars() {
            const progressContainers = document.querySelectorAll('.project-progress-container');
            
            progressContainers.forEach(container => {
                const progressBar = container.querySelector('.project-progress-bar');
                const progressBarInner = container.querySelector('.project-progress-bar-inner');
                const progressHandle = container.querySelector('.progress-handle');
                const progressPercent = container.closest('.enhanced-project-section').querySelector('.progress-percent');
                
                let isDragging = false;
                
                // 초기 퍼센트 설정
                let percent = 45;
                updateProgressBar(percent);
                
                // 드래그 이벤트 등록
                if (progressHandle) {
                    progressHandle.addEventListener('mousedown', function(e) {
                        isDragging = true;
                        document.addEventListener('mousemove', onMouseMove);
                        document.addEventListener('mouseup', onMouseUp);
                        e.preventDefault();
                    });
                    
                    // 터치 이벤트 등록
                    progressHandle.addEventListener('touchstart', function(e) {
                        isDragging = true;
                        document.addEventListener('touchmove', onTouchMove);
                        document.addEventListener('touchend', onTouchEnd);
                        e.preventDefault();
                    });
                }
                
                // 바로 클릭으로 진행도 변경 가능
                progressBar.addEventListener('click', function(e) {
                    const rect = progressBar.getBoundingClientRect();
                    const x = e.clientX - rect.left;
                    percent = Math.round((x / rect.width) * 100);
                    updateProgressBar(percent);
                });
                
                // 마우스 움직임 처리
                function onMouseMove(e) {
                    if (!isDragging) return;
                    
                    const rect = progressBar.getBoundingClientRect();
                    let x = e.clientX - rect.left;
                    
                    // 경계 체크
                    if (x < 0) x = 0;
                    if (x > rect.width) x = rect.width;
                    
                    percent = Math.round((x / rect.width) * 100);
                    updateProgressBar(percent);
                }
                
                // 마우스 업 이벤트
                function onMouseUp() {
                    isDragging = false;
                    document.removeEventListener('mousemove', onMouseMove);
                    document.removeEventListener('mouseup', onMouseUp);
                }
                
                // 터치 이동 이벤트
                function onTouchMove(e) {
                    if (!isDragging) return;
                    
                    const rect = progressBar.getBoundingClientRect();
                    let x = e.touches[0].clientX - rect.left;
                    
                    if (x < 0) x = 0;
                    if (x > rect.width) x = rect.width;
                    
                    percent = Math.round((x / rect.width) * 100);
                    updateProgressBar(percent);
                }
                
                // 터치 종료 이벤트
                function onTouchEnd() {
                    isDragging = false;
                    document.removeEventListener('touchmove', onTouchMove);
                    document.removeEventListener('touchend', onTouchEnd);
                }
                
                // 진행 바 업데이트 함수
                function updateProgressBar(percent) {
                    progressBarInner.style.width = `${percent}%`;
                    progressHandle.style.left = `${percent}%`;
                    progressPercent.textContent = `${percent}%`;
                }
            });
        }
        
        // 작업 상태 변경 함수
        function changeTaskStatus(selectElement) {
            // 모든 상태 클래스 제거
            selectElement.classList.remove('status-planned', 'status-in-progress', 'status-completed');
            
            // 선택된 값에 따라 클래스 추가
            const selectedValue = selectElement.value;
            if (selectedValue === 'planned') {
                selectElement.classList.add('status-planned');
            } else if (selectedValue === 'in-progress') {
                selectElement.classList.add('status-in-progress');
            } else if (selectedValue === 'completed') {
                selectElement.classList.add('status-completed');
            }
        }
        
        // 모달 관련 설정
        function setupModals() {
            const newProjectModal = document.getElementById('newProjectModal');
            const openNewProjectModalBtn = document.getElementById('openNewProjectModal');
            const closeNewProjectModalBtn = document.getElementById('closeNewProjectModal');
            const cancelNewProjectBtn = document.getElementById('cancelNewProject');
            
            const deleteConfirmModal = document.getElementById('deleteConfirmModal');
            const closeDeleteConfirmModalBtn = document.getElementById('closeDeleteConfirmModal');
            const cancelDeleteProjectBtn = document.getElementById('cancelDeleteProject');
            const confirmDeleteProjectBtn = document.getElementById('confirmDeleteProject');
            
            // 새 프로젝트 모달 열기
            openNewProjectModalBtn.addEventListener('click', function() {
                newProjectModal.classList.add('active');
            });
            
            // 새 프로젝트 모달 닫기
            closeNewProjectModalBtn.addEventListener('click', function() {
                newProjectModal.classList.remove('active');
            });
            
            cancelNewProjectBtn.addEventListener('click', function() {
                newProjectModal.classList.remove('active');
            });
            
            // 삭제 확인 모달 닫기
            closeDeleteConfirmModalBtn.addEventListener('click', function() {
                deleteConfirmModal.classList.remove('active');
            });
            
            cancelDeleteProjectBtn.addEventListener('click', function() {
                deleteConfirmModal.classList.remove('active');
            });
            
            // 삭제 확인
            confirmDeleteProjectBtn.addEventListener('click', function() {
                if (currentProjectToDelete) {
                    deleteProject(currentProjectToDelete);
                }
                deleteConfirmModal.classList.remove('active');
            });
            
            // 모달 바깥 클릭시 닫기
            newProjectModal.addEventListener('click', function(e) {
                if (e.target === newProjectModal) {
                    newProjectModal.classList.remove('active');
                }
            });
            
            deleteConfirmModal.addEventListener('click', function(e) {
                if (e.target === deleteConfirmModal) {
                    deleteConfirmModal.classList.remove('active');
                }
            });
        }
        
        // 프로젝트 관리 액션 설정
        function setupProjectActions() {
            // 이미 설정되어 있음 (이벤트 위임)
        }
        
        // 삭제 확인 모달 열기
        function confirmDeleteProject(projectId) {
            currentProjectToDelete = projectId;
            document.getElementById('deleteConfirmModal').classList.add('active');
        }
        
        // 프로젝트 삭제
        function deleteProject(projectId) {
            const projectElement = document.querySelector(`.enhanced-project-section[data-project-id="${projectId}"]`);
            if (projectElement) {
                projectElement.remove();
                
                // 프로젝트가 모두 삭제되었는지 확인
                const remainingProjects = document.querySelectorAll('.enhanced-project-section');
                if (remainingProjects.length === 0) {
                    // 모든 프로젝트가 삭제된 경우 안내 메시지 표시
                    const projectsContainer = document.getElementById('projectsContainer');
                    projectsContainer.innerHTML = `
                        <div style="text-align: center; padding: 30px; color: #6c757d; background: #f8f9fa; border-radius: 15px;">
                            <p>등록된 프로젝트가 없습니다.</p>
                            <p style="font-size: 14px; margin-top: 10px;">위의 '새 프로젝트 추가' 버튼을 클릭하여 새 프로젝트를 추가해보세요.</p>
                        </div>
                    `;
                }
            }
        }
        
        // 새 프로젝트 추가 함수
        function addNewProject() {
            const projectName = document.getElementById('projectName').value;
            const startDate = document.getElementById('startDate').value;
            const endDate = document.getElementById('endDate').value;
            const projectDescription = document.getElementById('projectDescription').value;
            
            if (!projectName) {
                alert('프로젝트명을 입력해주세요.');
                return;
            }
            
            // 프로젝트 ID 생성 (현재 시간 기반)
            const projectId = 'project-' + Date.now();
            
            // 새 프로젝트 HTML 생성
            const newProjectHTML = `
                <div class="enhanced-project-section" data-project-id="${projectId}">
                    <div class="project-header">
                        <h3 class="project-title">${projectName}</h3>
                        <div class="project-actions">
                            <button class="delete-project-btn" onclick="confirmDeleteProject('${projectId}')">삭제</button>
                        </div>
                    </div>
                    
                    <div style="display: flex; justify-content: space-between; font-size: 12px; margin-bottom: 5px;">
                        <span>프로젝트 진행도</span>
                        <span class="progress-percent">0%</span>
                    </div>
                    
                    <div class="project-progress-container">
                        <div class="project-progress-bar">
                            <div class="project-progress-bar-inner" style="width: 0%"></div>
                        </div>
                        <div class="progress-handle" style="left: 0%"></div>
                    </div>
                    
                    <div style="display: flex; justify-content: space-between; margin: 10px 0; font-size: 12px; color: #6c757d;">
                        <div>시작일: ${formatDate(startDate)}</div>
                        <div>종료 예정일: ${formatDate(endDate)}</div>
                    </div>
                    
                    <h4 style="font-weight: 500; margin: 15px 0 10px 0;">오늘의 작업</h4>
                    <div class="project-task-container">
                        <div class="project-task">
                            <div class="project-task-header">
                                <input type="text" placeholder="작업 내용" style="border: none; background: transparent; outline: none; font-weight: 500; width: 70%;">
                                <select class="task-status-select status-planned" onchange="changeTaskStatus(this)">
                                    <option value="planned" selected>계획됨</option>
                                    <option value="in-progress">진행중</option>
                                    <option value="completed">완료</option>
                                </select>
                            </div>
                            <textarea placeholder="상세 내용 및 얻은 인사이트" style="width: 100%; border: none; resize: none; outline: none; font-size: 14px; min-height: 60px;"></textarea>
                        </div>
                    </div>
                    
                    <h4 style="font-weight: 500; margin: 20px 0 10px 0;">프로젝트 회고</h4>
                    <textarea placeholder="오늘의 프로젝트에서 배운 점, 어려웠던 점, 내일 개선할 점 등을 기록하세요." style="width: 100%; border: 1px solid #dee2e6; border-radius: 10px; padding: 12px; min-height: 100px; outline: none; resize: none;">${projectDescription}</textarea>
                </div>
            `;
            
            // 프로젝트 컨테이너에 새 프로젝트 추가
            const projectsContainer = document.getElementById('projectsContainer');
            projectsContainer.insertAdjacentHTML('afterbegin', newProjectHTML);
            
            // 모달 닫기
            document.getElementById('newProjectModal').classList.remove('active');
            
            // 폼 리셋
            document.getElementById('newProjectForm').reset();
            
            // 새로 추가된 프로젝트의 진행도 바 설정
            setupProgressBars();
        }
        
        // 날짜 포맷 함수
        function formatDate(dateString) {
            const date = new Date(dateString);
            const year = date.getFullYear();
            const month = String(date.getMonth() + 1).padStart(2, '0');
            const day = String(date.getDate()).padStart(2, '0');
            return `${year}.${month}.${day}`;
        }
    </script>
</body>
</html>
